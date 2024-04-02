1.  Parte prática:
Requisitos AWS:
1-Gerar uma chave pública para acesso ao ambiente;

1.1-Vá ao painel EC2/Rede e segurança/Pares de Chaves, no lado superior direito, clique e criar par de chaves.
 ![image](https://github.com/AlonsoNeto01/Projeto-CompassAWS/assets/164195128/d216be1d-3f15-43a1-b6af-2bf18520a985)

1.2-Feita essa configuração, aperte no canto inferior “Criar par de chaves”.

2-Criar 1 instância EC2 com o sistema operacional Amazon Linux 2 (Família t3.small, 16 GB SSD);

2.1-No menu inicial, pesquise por EC2 e clique nele, vá na opção “instâncias” e no canto superior direito “Executar Instâncias”:

2.2-Na configuração da instância coloque as tags:

2.3-Escolha a imagem (no caso estaremos usando a Amazon Linux 2 AMI(HVM):
 ![image](https://github.com/AlonsoNeto01/Projeto-CompassAWS/assets/164195128/1537db65-fd69-48fd-b0b2-bf8442f1137a)

2.4-No tipo de instância usaremos a t3.small:
 ![image](https://github.com/AlonsoNeto01/Projeto-CompassAWS/assets/164195128/609320a9-a57a-4ed5-9b35-2b45e208caf7)

 
2.5-Par de chaves utilizaremos a que criamos na primeira etapa:

2.6-Em configurações de Rede, selecione a opção Criar Grupo de segurança:

2.7-Ainda nessa seção, deixe marcada a opção permitir tráfego SSH de Qualquer Lugar (0.0.0.0/0):

 ![image](https://github.com/AlonsoNeto01/Projeto-CompassAWS/assets/164195128/374eaf9e-f287-466b-9932-54b7b4559447)

2.8-Em Configurar Armazenamento, coloque 16gb de armazenamento gp2(SSD):

2.9-Revise as configurações e clique em Executar Instância.

3-Gerar 1 elastic IP e anexar à instância EC2;

3.1-Na página do serviço EC2, no menu lateral esquerdo, em Rede e Segurança, clique em IPs elásticos:

3.2-Clique em Alocar endereço IP elástico:

3.3-Por padrão, o Grupo de borda de Rede já vem selecionado, assim como o Conjunto de endereços IPv4 públicos da Amazon: Clique em Alocar: 

3.4-Depois da criação, selecione o IP na lista, clique em Ações no menu superior e depois em Associar endereço IP elástico:

3.5-Selecione a instância EC2 criada anteriormente: Depois de selecionar a instância será preciso selecionar o endereço IP privado, que será sugerido pela própria plataforma, bastando confirmar:

3.6-Marque a opção permitir que o endereço IP elástico seja reassociado e clique em Associar.
 ![image](https://github.com/AlonsoNeto01/Projeto-CompassAWS/assets/164195128/f61d37d8-2390-4353-8986-eaf2dfbf120d)


4-Liberar as portas de comunicação para acesso público: (22/TCP, 111/TCP e UDP, 2049/TCP/UDP, 80/TCP, 443/TCP).

4.1-Na página do serviço EC2, no menu lateral esquerdo, em Rede e Segurança, clique em Security groups; Selecione o grupo de segurança que foi criado com a instância EC2;

4.2-Clique em Regras de entrada, na parte inferior, e depois, do lado direito da tela, em Editar regras de entrada;

4.3-Por padrão, já temos uma regra de entrada, do Tipo SSH, no Intervalo de portas 22, Protocolo TCP. Essa regra será mantida;

4.4-Clique em Adicionar regras. Agora iremos acrescentar a liberação de outras portas, além da 22 que já consta, conforme indicado na tabela abaixo:
 ![image](https://github.com/AlonsoNeto01/Projeto-CompassAWS/assets/164195128/8c55ecee-396a-4231-8c03-c970a3c1e788)

Requisitos no linux:

1-Configurar o NFS entregue;

1.1-Para configurarmos um servidor NFS na máquina Linux nos próximos passos, vamos utilizar o serviço EFS da própria AWS. Antes, vamos configurar um grupo de segurança que será utilizada para a rede do EFS mais adiante.

1.2-Vá até o Painel EC2 da AWS e clique em Security groups:

1.3-Clique em Criar grupo de segurança:

1.4-Atribua um nome:

1.5-Selecione a mesma VPC em que se encontra a instância. Ela aparecerá listada para você:

1.6-Em Regras de entrada adicione uma nova regra seguindo o modelo abaixo:
 ![image](https://github.com/AlonsoNeto01/Projeto-CompassAWS/assets/164195128/abeeace7-213f-4cf0-a770-18011d40e0ba)

1.7-Quando for escolher o campo Origem, escolha a opção Personalizado e, na caixa ao lado, role a barra até encontrar o grupo de segurança que foi criado para a instância EC2 que vamos acessar. Dessa forma, os dois grupos de segurança estarão conectados, cada um com seu objetivo:

1.8-Clique em Criar grupo de segurança para finalizar.

2-Criar um diretorio dentro do filesystem do NFS com seu nome;

2.1-No console AWS, navegue até o serviço de EFS:

2.2-No menu lateral esquerdo, clique em Sistemas de arquivos e, na sequência, em Criar sistema de arquivos:

2.3-Adicione um nome para o sistema de arquivos(no caso utilizaremos meu nome) e selecione a opção Personalizar:

2.4-Marque a opção One zone e selecione a mesma zona de disponibilidade em que sua instância foi criada e avance:

2.5-Mantenha as opções pré-definidas, altere apenas o grupo de segurança para o grupo que criamos para o serviço EFS:

2.6-Revise as informações e clique em Criar para terminar:

2.7-Na lista de sistemas criados, abra o sistema de arquivos recém-feito e clique no botão Anexar para visualizar as opções de montagem (IP ou DNS):

2.8-A AWS já nos apresenta comandos definidos de acordo com as opções escolhidas. Aqui, vamos utilizar a montagem via DNS usando o cliente do NFS. Copie-o e salve em um bloco de notas, pois irá precisar dele mais adiante. O comando segue o seguinte modelo: ”sudo mount -t nfs4 -o nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2 [DNS do EFS]:/ [caminho local]”:


#Como estou usando o terminal da AWS ele faz a conexão direta via SSH.#


2.9-A partir de agora nossas ações serão feitas no terminal Linux da instância EC2.Caso necessário, entre com o comando “sudo su” para ganhar privilégios administrativos:
Execute o comande de atualização do sistema “sudo yum update -y” antes de iniciar instalações, para garantir que serão sempre as versões mais atualizadas dos arquivos Linux que rodarão:

2.10-Com o comando “sudo yum install -y amazon-efs-utils” instale o pacote para suporte ao NFS. É um protocolo que permite compartilhar diretórios e arquivos entre sistemas operacionais em uma rede:

2.11-Utilize o comando “sudo mkdir /mnt/efs” para criar um diretório local que servirá como ponto de montagem:

2.12-Agora vamos montar o sistema de arquivos. Para isso, é preciso utilizar o comando que foi copiado anteriormente, “sudo mount -t nfs4 -o nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2 [DNS do EFS]:/ [caminho local]”, lembrando que cada um terá o seu próprio DNS, disponibilizado nos detalhes do serviço na AWS, e caminho local, que aqui foi nomeado como /mnt/efs.



3-Subir um apache no servidor - o apache deve estar online e rodando;

3.1-Atualize os pacotes do sistema com o comando "sudo yum update -y":

3.2-Instale o Apache com o comando "sudo yum install httpd -y":
 ![image](https://github.com/AlonsoNeto01/Projeto-CompassAWS/assets/164195128/c069f68e-a71d-4b17-a010-e9cb9fb68d7f)

3.3-Inicie o Apache no sistema com o comando “sudo systemctl start httpd” ou ainda o “sudo /bin/systemctl start httpd.service”:

3.4-Para o Apache iniciar automaticamente, execute o comando “sudo systemctl enable httpd”:

3.5-Verifique se o apache está em execução através do comando “sudo systemctl status httpd”:
 ![image](https://github.com/AlonsoNeto01/Projeto-CompassAWS/assets/164195128/f1ef77df-cac5-4ac7-81f5-922d4db01081)

O Apache já vem com uma página inicial padrão que pode ser acessada através da digitação do IP público na barra de endereço de um navegador. Mas também é possível editar essa página HTML para que exiba o que você quiser. Isso é feito a partir de um arquivo index que pode ser criado dentro do diretório do Apache:
 ![image](https://github.com/AlonsoNeto01/Projeto-CompassAWS/assets/164195128/f037074b-b5a4-48dd-a0ee-1f146401c0de)

3.6-Para criar/editar esse arquivo, digite o comando “sudo nano index.html”. O arquivo HTML que você digitar nesse documento é o que será mostrado na página acessada pelo IP público. Veja a seguir um exemplo de documento HTML para o serviço:
 ![image](https://github.com/AlonsoNeto01/Projeto-CompassAWS/assets/164195128/1f078b72-2854-4f41-a6f9-14a9b1a4bb8b)

3.7-Para salvar o documento no editor nano, aperte ctrl+x, depois y e confirme apertando enter:

3.8-Para acessar a página e ver se funcionou, basta colar o IP público da instância (informação disponível nos detalhes da instância na AWS) na barra de endereço de um navegador:
 ![image](https://github.com/AlonsoNeto01/Projeto-CompassAWS/assets/164195128/8581d34a-7a89-4b03-94a8-6cb3b22c6435)

4-Criar um script que valide se o serviço esta online e envie o resultado da validação para o seu diretorio no nfs. O script deve conter - Data HORA + nome do serviço + Status + mensagem personalizada de ONLINE ou offline;

Para criar um script será necessário utilizar um editor de texto (utilizaremos o nano) e, ao final do nome do arquivo, devemos atribuir a extensão .sh. Devemos lembrar que, para essa atividade, o script deve conter data, hora, nome do serviço, status e mensagem personalizada de ONLINE ou OFFLINE.
O script também deve gerar 2 arquivos de saída: um para o serviço online e outro para o serviço offline.

4.1-Execute o comando “nano service_status.sh” para criar e abrir o arquivo do script. É importante criar o script dentro do diretório EFS. Aqui vamos salvá-lo no caminho /mnt/efs/Alonso:

4.2-Dentro do arquivo, digite o script desejado. O script criado para essa atividade pode ser observado na imagem a seguir:
 ![image](https://github.com/AlonsoNeto01/Projeto-CompassAWS/assets/164195128/296f110e-ac0e-4339-a495-7cd25e9435ea)

Note que, no exemplo acima, dentro do esquema "if/else", já indicamos que a operação deve criar, no caminho do diretório indicado, e enviar dois arquivos em formato .txt com os resultados da verificação. Sendo um arquivo para o resultado online e outro para o resultado offline:

4.3-Para tornar o arquivo do script executável digite o comando “sudo chmod +x [nome do script]”, sendo, nesse caso, “sudo chmod +x service_status.sh”:

4.4-Estando no diretório onde o script foi criado e ativado, execute o comando “./service_status.sh” para executá-lo. Caso esteja funcionando corretamente e o serviço esteja online, o script vai criar o documento .txt que guarda as informações da validação online:

Esse documento pode ser lido com o comando cat + nome do documento: “cat httpd-online.txt”. É possível verificar o funcionamento do script na imagem abaixo:
 ![image](https://github.com/AlonsoNeto01/Projeto-CompassAWS/assets/164195128/4a63f04a-bcd5-4784-9352-1b6758f3bb38)

Note que o documento informa a data e a hora em que a verificação foi feita, assim como o nome do serviço verificado e uma mensagem indicando que o mesmo está online.


5-O script deve gerar 2 arquivos de saida: 1 para o serviço online e 1 para o serviço OFFLINE. Preparar a execução automatizada do script a cada 5 minutos.

Para o agendamento da execução do script vamos utilizar o comando crontab. Normalmente o crontab abre um arquivo com o programa vi de edição de texto. Sendo o vi não muito prático, é possível modificar para que a abertura ocorra com o nano, muito mais intuitivo e semelhante aos editores de texto convencionais.

5.1-Digite o comando “EDITOR=nano crontab -e”, para que o nano abra o arquivo crontab:

5.2-Dentro do arquivo digite a linha “*/5 * * * * /[caminho de onde está o script/nome do script]”. No meu caso, ficou dessa forma: “*/5 * * * * /mnt/efs/Alonso/service_status.sh”:

Salve o arquivo e feche o editor:

5.3-Para verificar se a automatização está funcionando, é preciso abrir os arquivos .txt que foram programados para serem criados e guardar as informações da verificação do serviço online e offline. Como a automatização faz com que a verificação programada pelo script ocorra a cada 5 minutos, dê algum tempo para que o arquivo .txt seja atualizado algumas vezes:

5.4-Na imagem abaixo temos a demonstração do arquivo httpd-online.txt exibindo as informações da validação online após o crontab realizar a automatização algumas vezes:
![image](https://github.com/AlonsoNeto01/Projeto-CompassAWS/assets/164195128/869072a4-056d-4ef3-a504-5e78e299a803)

5.5-Para fazermos a confirmação de que o script realiza a verificação do serviço offline é preciso interromper o Apache com o comando sudo systemctl stop httpd. Dessa forma, basta aguardar alguns minutos para que o crontap continue a executar o script a cada 5 minutos e poderemos ver a criação do arquivo httpd-offline.txt, que exibe os momentos em que o status do serviço estava offline, conforme imagem abaixo:
![image](https://github.com/AlonsoNeto01/Projeto-CompassAWS/assets/164195128/8a871b1f-0365-4572-a3fa-7c643b35d100)

Ainda, é possível verificarmos que os arquivos .txt foram criados dentro do diretório indicado no script:
![image](https://github.com/AlonsoNeto01/Projeto-CompassAWS/assets/164195128/e4872579-5ffc-4265-bbad-1a0d8ac81cae)

•	E no final de tudo encerrar a instância!!! 
![image](https://github.com/AlonsoNeto01/Projeto-CompassAWS/assets/164195128/11a2a804-480d-465d-8e15-bf740c2d039f)



