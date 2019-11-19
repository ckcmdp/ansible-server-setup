# Rails App Deployment through Ansible

The playbook deploy.yaml can deploy a rails app.
Please follow the below steps to setup accordingly.

1. Check the ansible.cfg file and update the 'inventory' entry.
   eg:  *inventory = /home/username/dev/ansible-server/hosts.yaml*

2. Check the hosts.yaml file and change the lines as shown in the example.
   eg:-
  ```
        servers:
          hosts:
            ubun:
              ansible_connection: ssh
              ansible_host: "localhost"     <<-- provide your server ip
              ansible_port: 52022           <<-- provide your server port
              ansible_user: 'root'

        local_machine:
          hosts:
            my_system:
              ansible_connection: ssh
              ansible_host: "localhost"
              ansible_port: 22
              ansible_user: 'Arnab'         <<-- provide your local machine username
  ```
   Note: Don't change any other values but you can add all your host servers under the group **servers**. In this way you can deploy to multiple servers but remember to configure your production.rb in proper way so that capistrano can deploy to all the servers.

3. Add your ssh key( id_rsa.pub content ) to authorized_keys file for setting up passwordless login within your own machine. Check it by running the command on your local machine
   ```
   ssh 'yourusername'@localhost -p 22
   ```

4. Check the variables.yml file and provide the proper values for each and every variable. Details of variables are provided in the variables.yml file itself.

5. In your app's production.rb check the deploy_to path and update it according to the path present in variables.yml file

6. Run the following command to ensure that all hosts are accessible.
   ```
    ansible -m ping all
   ```

7. Now run the playbook.
   ```
    ansible-playbook deploy.yaml
   ```