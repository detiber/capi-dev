enable_feature("snapshots")

allow_k8s_contexts('kubernetes-admin@kubernetes')

settings = read_json('config.json', default={})
default_registry(settings.get('default_registry'))
default_repo_root(settings.get('default_repo_root'))

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
        'repo': 'default_repo_root' + core_provider,
    },
    {
        'name': bootstrap_provider,
        'image': bootstrap_image,
        'repo': 'default_repo_root' + bootstrap_provider,
    },
    {
        'name': docker_provider,
        'image': docker_image,
        'repo': 'default_repo_root' + docker_provider,
    },
    {
        'name': aws_provider,
        'image': aws_image,
        'repo': 'default_repo_root' + aws_provider,
    },
]

for provider in providers:
    command = '''sed -i -e 's@image: .*@image: '"{}"'@' {}/config/default/manager_image_patch.yaml'''.format(provider['image'], provider['repo'])
    local(command)
    kustomizedir = provider['repo'] + '/config/default'
    # listdir(kustomizedir)
    k8s_yaml(kustomize(kustomizedir))

# core capi
docker_build(core_image, 'default_repo_root' + core_provider)

# bootstrap provider
docker_build(bootstrap_image, 'default_repo_root' + bootstrap_provider)

# aws provider
docker_build(docker_image, 'default_repo_root' + aws_provider)

# docker provider
docker_build(docker_image, 'default_repo_root' + docker_provider)
