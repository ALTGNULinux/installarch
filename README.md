# Instalando Arch Linux e aprendendo sobre Linux no Processo
==========
![ArchLinux](https://github.com/ALTGNULinux/installarch/blob/master/src/ArchLinux.png)

1 - Obtendo a ISO de instalação do Arch Linux
==========
Você pode obter a ISO de instalação do Arch na [pagina de download] (https://www.archlinux.org/download/). Como trata-se de uma distribuição Rolling Release, é gerada uma ISO atualizada mensalmente. Após obter a instalação, grave a ISO em um CD ou Pen Drive e inicie a instalação.

![ArchLinux](https://github.com/ALTGNULinux/installarch/blob/master/src/1.1.png)

Após iniciar a instalação, verifique se você tem conexão com a internet (utilizarei conexão cabeada durante o procedimento). Teste com o seguinte comando:
>
	ping 8.8.8.8 -c5

Caso você ainda não tenha conexão com a internet ative o dhcpcd com o comando:
>
	systemctl start dhcpcd

Caso deseje conectar-se via Wireless utilize o comando `wifi-menu`.
 
2 - Configurando as partições
==========
Existem várias formas e ferramentas para se particionar o disco. Iremos utilizar a ferramenta `Cfdisk` para criar as partições, e irei abordar a forma de particionamento para os dois tipos de tabelas de partições (GPT e MBR).

**Tenha cuidado nessa parte!**

***Em seguida particione o seu disco de acordo com o tipo que você utilizar, GPT ou MBR (estão separadas por partes no tutorial 2.1 e 2.2).***


Inicie o Cfdisk:
>
	cfdisk
A interface de particionamento é essa:
![ArchLinux](https://github.com/ALTGNULinux/installarch/blob/master/src/2.2.png)


### 2.1 MBR - BIOS

Sera criada a seguinte partição:
>
	/dev/sda1	 como /		sera bootavel e com o tamanho que desejar

No `cfdisk` devera ficar da seguinte forma:
![ArchLinux2](https://github.com/ALTGNULinux/installarch/blob/master/src/2.3.png)

Se tudo estiver correto grave as alterações no disco.

Agora iremos formatar e montar a partição criada.

Execute os seguintes comandos:
>
	mkfs.ext4 /dev/sda1  (A partição selecionada será formatada em ext4)
	mount /dev/sda1 /mnt (Será montada a partição em /mnt)
	mkdir /mnt/home (Sera criada a home dentro de /mnt)

### 2.2 GPT - UEFI

Serão criadas as seguintes partições:
>
	/dev/sda1	como /boot		com o tamanho de 512M
	/dev/sda2	como /			com o tamanho que desejar

No `cfdisk` devera ficar da seguinte forma:
![ArchLinux2](https://github.com/ALTGNULinux/installarch/blob/master/src/2.4.png)

Se tudo estiver correto grave as alterações no disco.

Agora iremos formatar e montar a partição criada.

Execute os seguintes comandos:
>
	mkfs.vfat -F32 /dev/sda1  (Formata e prepara a partição de boot)
	mkfs.ext4 /dev/sda2 (A partição selecionada será formatada em ext4)
	mount /dev/sda2 /mnt (Será montada a partição sda2 em /mnt)
	mkdir /mnt/boot (Será criada a pasta boot dentro de /mnt)
	mkdir /mnt/home (Será criada a pasta home dentro de /mnt)
	mount /dev/sda1 /mnt/boot (Monta a partição de boot)

3 - Instalando o Sistema Base do Arch Linux
==========
Iremos agora instalar a base do sistema. Caso sua internet for lenta sugiro um café! =D

Execute o seguinte comando:
>
	pacstrap /mnt base base-devel

O Download ira começar...

![ArchLinux](https://github.com/ALTGNULinux/installarch/blob/master/src/3.1.png)

Aguarde o processo ser finalizado...

![ArchLinux](https://github.com/ALTGNULinux/installarch/blob/master/src/3.2.png)

Em seguida devemos gerar o nosso fstab que significa File System Table, que fica localizado em /etc/fstab. Ele define como as partições do disco e outros dispositivos serão montados. 

Iremos utilizar uma ferramenta do sistema para gerar o fstab automaticamente.

Para gerá-lo execute:
>
	genfstab /mnt >> /mnt/etc/fstab

Caso desejar você pode visualizar o arquivo ou edita-lo com:
>
	cat /mnt/etc/fstab
	nano /mnt/etc/fstab

Arquivo gerado pelo genfstab:
![ArchLinux](https://github.com/ALTGNULinux/installarch/blob/master/src/3.3.png)

4 - Configurando o Sistema
==========
Após termos instalado o sistema e gerado o fstab, agora devemos realizar algumas configurações basicas.
Para isso devemos acessar o sistema, com o seguinte comando:
>
	arch-chroot /mnt

Após executar o comando deverá ficar assim:
![ArchLinux](https://github.com/ALTGNULinux/installarch/blob/master/src/4.1.png)

Agora devemos configurar o locales, para definir o nosso idioma e layout do teclado.
Abra o arquivo /etc/locale.gen e remova o # da frente de pt_BR.UTF-8 UTF-8, ou referente ao idioma do seu teclado.
>
	nano /etc/locale.gen (Remova # do inicio da linha referente ao idioma desejado)
	locale-gen

Após realizar as modificações salve o arquivo.
![ArchLinux](https://github.com/ALTGNULinux/installarch/blob/master/src/4.2.png)

Em seguida iremos configurar o nosso idioma, criando o arquivo /etc/locale.conf (caso deseje utilizar um idioma diferente adapte o comando de acordo com o desejado).
Execute o comando, para definir o nosso idioma:
>
	echo LANG=pt_BR.UTF-8 > /etc/locale.conf
	export LANG=pt_BR.UTF-8

Foi criado o arquivo com os parametros passados no comando:
![ArchLinux](https://github.com/ALTGNULinux/installarch/blob/master/src/4.3.png)

Agora vamos configurar o fuso horário.
Podem ser vistas, as regiões do Brazil com o seguinte comando:
>
	ls /usr/share/zoneinfo/Brazil

No meu caso irei utilizar East, execute o comando:
>
	ln -s /usr/share/zoneinfo/Brazil/East /etc/localtime

Vamos definir um hostname para a maquina:
>
	echo Arch > /etc/hostname

E definir uma senha de root:
>
	passwd
