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
Marque a opção One zone e selecione a mesma zona de disponibilidade em que sua instância foi criada e avance: Mantenha as opções pré-definidas, altere apenas o grupo de segurança para o grupo que criamos para o serviço EFS: Revise as informações e clique em Criar para terminar: Na lista de sistemas criados, abra o sistema de arquivos recém-feito e clique no botão Anexar para visualizar as opções de montagem (IP ou DNS):
A AWS já nos apresenta comandos definidos de acordo com as opções escolhidas. Aqui, vamos utilizar a montagem via DNS usando o cliente do NFS. Copie-o e salve em um bloco de notas, pois irá precisar dele mais adiante. O comando segue o seguinte modelo: ”sudo mount -t nfs4 -o nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2 [DNS do EFS]:/ [caminho local]”:


Como estou usando o terminal da AWS ele faz a conexão direta via SSH.


A partir de agora nossas ações serão feitas no terminal Linux da instância EC2.Caso necessário, entre com o comando “sudo su” para ganhar privilégios administrativos:
Execute o comande de atualização do sistema “sudo yum update -y” antes de iniciar instalações, para garantir que serão sempre as versões mais atualizadas dos arquivos Linux que rodarão:
Com o comando “sudo yum install -y amazon-efs-utils” instale o pacote para suporte ao NFS. É um protocolo que permite compartilhar diretórios e arquivos entre sistemas operacionais em uma rede:
Utilize o comando “sudo mkdir /mnt/efs” para criar um diretório local que servirá como ponto de montagem:


