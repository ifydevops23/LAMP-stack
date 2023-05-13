# WEB STACK IMPLEMENTATION (LEMP STACK)

In this project, I implemented a similar stack to LAMP, but with an alternative Web Server – NGINX, which is also very popular and widely used by many websites on the Internet.
The acronym LEMP represents Linux, eNginx, MySQL, PHP

## STEP 0 - Requirements
- An Ubuntu Virtual Machine

![0_PreReq_Ubuntu_VM](https://github.com/ifydevops23/Software_Stack/assets/126971054/9391c565-7df0-4cff-a6a2-5813f38ac739)

- SSH client for connecting to the VM

## STEP 1 - INSTALLING THE NGINX WEB SERVER
- Update the server package index by typing `sudo apt update`.
- Use `apt install` to get Nginx installed.

![1_install_nginx](https://github.com/ifydevops23/Software_Stack/assets/126971054/2420e30a-b2cc-4272-b6a9-dfcb9c49d8f2)

- Verify that nginx was successfully installed and is running as a service in Ubuntu `sudo systemctl status nginx`.

![1_nginx_status](https://github.com/ifydevops23/Software_Stack/assets/126971054/7ab9c751-e069-4164-a28a-dadcce579951)

- To access it locally in our Ubuntu shell, run `curl http://localhost:80` or curl [http://127.0.0.1:80] via a web browser.

![1_success_from_curl1a](https://github.com/ifydevops23/Software_Stack/assets/126971054/fcdbb8e5-15d5-4e7c-9880-189b19606f6f)

## STEP 2 — INSTALLING MYSQL







