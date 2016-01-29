# Como Instalar Arch Linux e Aprender sobre Linux no Processo
==========
![ArchLinux](https://github.com/ALTGNULinux/installarch/blob/master/src/ArchLinux2.png)

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

***Tenha cuidado com o numero das partições, que para você possivelmente seja diferente!***


Inicie o Cfdisk:
>
	cfdisk
A interface de particionamento é essa:
![ArchLinux](https://github.com/ALTGNULinux/installarch/blob/master/src/2.2.png)


### 2.1 MBR - BIOS
==========
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
==========
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

3 - Instalação do Sistema Base do Arch Linux
==========
Iremos agora instalar a base do sistema. Caso sua internet for lenta sugiro um café! =D

Execute o seguinte comando:
>
	pacstrap /mnt base base-devel

![ArchLinux](https://github.com/ALTGNULinux/installarch/blob/master/src/3.1.png)

E aguarde o processo ser finalizado...

![ArchLinux](https://github.com/ALTGNULinux/installarch/blob/master/src/3.2.png)

Em seguida devemos gerar o nosso fstab que significa File System Table, que fica localizado em /etc/fstab. Ele define como as partições do disco e outros dispositivos serão montados para o sistema de arquivo. Vamos utilizar uma ferramenta do sistema para gerar o fstab automaticamente.

Para gera-lo execute:
>
	genfstab /mnt >> /mnt/etc/fstab

Caso desejar você pode visualizar o arquivo ou edita-lo com:
>
	cat /mnt/etc/fstab
	nano /mnt/etc/fstab

Arquivo gerado pelo genfstab:
![ArchLinux](https://github.com/ALTGNULinux/installarch/blob/master/src/3.3.png)
