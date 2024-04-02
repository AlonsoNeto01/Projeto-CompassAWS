1.  Parte prática:
Requisitos AWS:
1-Gerar uma chave pública para acesso ao ambiente;
1.1-Vá ao painel EC2/Rede e segurança/Pares de Chaves, no lado superior direito, clique e criar par de chaves.
 ![image](https://github.com/AlonsoNeto01/Projeto-CompassAWS/assets/164195128/d216be1d-3f15-43a1-b6af-2bf18520a985)

1.2-Feita essa configuração, aperte no canto inferior “Criar par de chaves”.
•	Criar 1 instância EC2 com o sistema operacional Amazon Linux 2 (Família t3.small, 16 GB SSD);
No menu inicial, pesquise por EC2 e clique nele, vá na opção “instâncias” e no canto superior direito “Executar Instâncias”.
Na configuração da instância coloque as tags.
Escolha a imagem (no caso estaremos usando a Amazon Linux 2 AMI(HVM):
 
No tipo de instância usaremos a t3.small:
 
Par de chaves utilizaremos a que criamos na primeira etapa:
Em configurações de Rede, selecione a opção Criar Grupo de segurança:
Ainda nessa seção, deixe marcada a opção permitir tráfego SSH de Qualquer Lugar (0.0.0.0/0):
 
Em Configurar Armazenamento, coloque 16gb de armazenamento gp2(SSD):
Revise as configurações e clique em Executar Instância.
•	Gerar 1 elastic IP e anexar à instância EC2;
Na página do serviço EC2, no menu lateral esquerdo, em Rede e Segurança, clique em IPs elásticos: Clique em Alocar endereço IP elástico: Por padrão, o Grupo de borda de Rede já vem selecionado, assim como o Conjunto de endereços IPv4 públicos da Amazon: Clique em Alocar: Depois da criação, selecione o IP na lista, clique em Ações no menu superior e depois em Associar endereço IP elástico: Selecione a instância EC2 criada anteriormente: Depois de selecionar a instância será preciso selecionar o endereço IP privado, que será sugerido pela própria plataforma, bastando confirmar: Marque a opção permitir que o endereço IP elástico seja reassociado e clique em Associar.
 

•	Liberar as portas de comunicação para acesso público: (22/TCP, 111/TCP e UDP, 2049/TCP/UDP, 80/TCP, 443/TCP).
Na página do serviço EC2, no menu lateral esquerdo, em Rede e Segurança, clique em Security groups; Selecione o grupo de segurança que foi criado com a instância EC2;
Clique em Regras de entrada, na parte inferior, e depois, do lado direito da tela, em Editar regras de entrada;
Por padrão, já temos uma regra de entrada, do Tipo SSH, no Intervalo de portas 22, Protocolo TCP. Essa regra será mantida;
Clique em Adicionar regras. Agora iremos acrescentar a liberação de outras portas, além da 22 que já consta, conforme indicado na tabela abaixo:
 


