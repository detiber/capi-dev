enable_feature("snapshots")

allow_k8s_contexts('kubernetes-admin@kubernetes')

settings = read_json('config.json', default={})
default_registry(settings.get('default_registry'))

core_provider = 'cluster-api'
bootstrap_provider = 'cluster-api-bootstrap-provider-kubeadm'
docker_provider = 'cluster-api-provider-docker'
aws_provider = 'cluster-api-provider-aws'

core_image = settings.get('default_core_image')
bootstrap_image = settings.get('default_bootstrap_image')
docker_image = settings.get('default_docker_image')
aws_image = settings.get('default_aws_image')

providers = [
    {
        'name': core_provider,
        'image': core_image,
    },
    {
        'name': bootstrap_provider,
        'image': bootstrap_image,
    },
    {
        'name': docker_provider,
        'image': docker_image,
    },
#    {
#        'name': aws_provider,
#        'image': aws_image,
#    },
]

for provider in providers:
    command = '''sed -i -e 's@image: .*@image: '"{}"'@' ./{}/config/default/manager_image_patch.yaml'''.format(provider['image'], provider['name'])
    local(command)
    kustomizedir = './' + provider['name'] + '/config/default'
    # listdir(kustomizedir)
    k8s_yaml(kustomize(kustomizedir))

docker_build(core_image, '/home/detiber/go/src/sigs.k8s.io/cluster-api')

docker_build(bootstrap_image, '/home/detiber/go/src/sigs.k8s.io/cluster-api-bootstrap-provider-kubeadm')

## Uncomment one of the two depending on which docker provider you are using

# # aws provider
# docker_build(docker_image, '/home/detiber/go/src/sigs.k8s.io/cluster-api-provider-aws')

# docker provider
docker_build(docker_image, '/home/detiber/go/src/sigs.k8s.io/cluster-api-provider-docker')
