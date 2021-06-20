pipeline{
agent
stages{
    stage(clone and check SCM){
          steps{
             git branch: '2021-06', url: 'https://github.com/janakirammeka/simplilearn-dockercoins.git'
           }
        }
    stage(build and test image){
        steps{
           sh "docker image build --file hasher/Dockerfile --tag index.docker.io/ramjmeka/simplilearn-dockercoins-2021-06:test-hasher hasher"
           sh "docker image build --file rng/Dockerfile --tag index.docker.io/ramjmeka/simplilearn-dockercoins-2021-06:test-rng rng"
           sh "docker image build --file webui/Dockerfile --tag index.docker.io/ramjmeka/simplilearn-dockercoins-2021-06:test-webui webui"
           sh "docker image build --file worker/Dockerfile --tag index.docker.io/ramjmeka/simplilearn-dockercoins-2021-06:test-worker worker"
           sh "docker network create hasher-network"
           sh "docker network create redis-network"
           sh "docker network create rng-network"
           sh "docker container run --detach --entrypoint docker-entrypoint.sh --name redis --network redis-network --volume redis-volume:/data --workdir /data redis:alpine redis-server"
           sh "docker container run --detach --entrypoint ruby --name hasher --network hasher-network --workdir /src index.docker.io/ramjmeka/simplilearn-dockercoins-2021-06:test-hasher hasher.rb"
           sh "docker container run --detach --entrypoint python --name rng --network rng-network --workdir /src index.docker.io/ramjmeka/simplilearn-dockercoins-2021-06:test-rng rng.py"
           sh "docker container run --detach --entrypoint node --name webui --network redis-network --workdir /src index.docker.io/ramjmeka/simplilearn-dockercoins-2021-06:test-webui webui.js"
           sh "docker container run --detach --entrypoint python --name worker --network redis-network --workdir /src index.docker.io/ramjmeka/simplilearn-dockercoins-2021-06:test-worker worker.py"
           sh "docker network connect hasher-network worker"
           sh "docker network connect rng-network worker"
           sh "while true ; do sleep 10 && docker container logs worker && docker container logs worker 2>& 1 | grep 'Coin found' && break ; done"
        }
    }
 }
}



