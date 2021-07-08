Usando como base a pasta do Docker  
(Caso a sua loja nÃ£o seja criada como magento2.local, Ã© necessÃ¡rio ajustar nos comandos para que fique de acordo)  
  
#1 - Copie nginx/conf.d/magento2.conf.example para nginx/conf.d/magento2.local.conf  
Caso sua loja nÃ£o seja magento2.local, serÃ¡ necessÃ¡rio editar a cÃ³pia para configurar os paths corretamente  
  
#2 - Crie a pasta magento2.local dentro da pasta var/www/  
  
#3 - Crie os certificados auto assinados, isso vai gerar depois uma mensagem no Chrome se vocáº½ confia no certificado, mas como vocÃª quem criou, pode confiar  
`openssl req -newkey rsa:2048 -nodes -keyout ./nginx/certificates/magento2.local.key -x509 -days 365 -out ./nginx/certificates/magento2.local.crt`  
  
#4 - Crie a entrada no /etc/hosts usando sudo   
`sudo echo "127.0.0.1 magento2.local" >> /etc/hosts`  

#4.1 - Colocar o host no windows também se estiver usando windows
Pasta C:\windows\System32\Drivers\Etc
Editar arquivo hosts igual o do linux
  
#5 - Copie suas credenciais SSH para dentro para dentro da mÃ¡quina PHP 7.4  
`cp ~/.ssh/* ./ssh-keys/`  
`sudo chmod 664 ./ssh-keys/*`  
  
#6 - Mude as variÃ¡veis do script magento.sh referentes Ã  nome e e-mail, por motivos Ã³bvios. As variÃ¡veis a serem trocadas sÃ£o:  
MAGENTO_BASE_DIR=/var/www/acc.local
MAGENTO_URL=https://acc.local
MAGENTO_BASE_URL_SECURE=https://acc.local
STORE_NAME="Local Store"
STORE_EMAIL=larry.de.a.junior@accenture.com
  
#7 - Dentro do servidor PHP rodar os comandos desse script, para entrar no servidor  
`docker-compose exec --user www-data php74-fpm bash`  
`cd magento2.local`  
  
#8 - Dentro do servidor php, verifique a versÃ£o do seu composer utilizando:  
`composer -V`  
Caso a versÃ£o do seu composer seja maior ou igual Ã  versÃ£o 2, Ã© necessÃ¡rio fazer o downgrade do composer para a versÃ£o 1.x  
O composer faz isso automaticamente utilizando  
`sudo composer self-update --1`  
  
#9 - Adicionar www-data como usuario no group e dar permissão na pasta var/www/
sudo usermod -a -G www-data `whoami`
sudo chmod 775 var/www/
sudo chmod 777 var/www/{pasta-do-projeto}
 OU 
sudo chmod -R 777 var/www/



