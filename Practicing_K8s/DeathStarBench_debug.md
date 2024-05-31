## socialNetwork

junho@junho-System-Product-Name:~/DeathStarBench/socialNetwork$ docker-compose build
Traceback (most recent call last):
File "/usr/bin/docker-compose", line 33, in <module>
sys.exit(load_entry_point('docker-compose==1.29.2', 'console_scripts', 'docker-compose')())
File "/usr/lib/python3/dist-packages/compose/cli/main.py", line 81, in main
command_func()
File "/usr/lib/python3/dist-packages/compose/cli/main.py", line 200, in perform_command
project = project_from_options('.', options)
File "/usr/lib/python3/dist-packages/compose/cli/command.py", line 60, in project_from_options
return get_project(
File "/usr/lib/python3/dist-packages/compose/cli/command.py", line 152, in get_project
client = get_client(
File "/usr/lib/python3/dist-packages/compose/cli/docker_client.py", line 41, in get_client
client = docker_client(
File "/usr/lib/python3/dist-packages/compose/cli/docker_client.py", line 124, in docker_client
kwargs = kwargs_from_env(environment=environment, ssl_version=tls_version)
TypeError: kwargs_from_env() got an unexpected keyword argument 'ssl_version'

That’s because the install script is pushing/installing an obsolete version of docker-compose and the system gets confused because docker ships with an updated version of it by default
I was able to fix it from my side by doing

pip remove docker-compose
apt reinstall docker-compose-plugin
curl -L https://umbrel.sh | bash -s -- --no-install-docker --no-install-compose
