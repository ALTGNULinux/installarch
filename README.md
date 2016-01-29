# Como Instalar Arch Linux e Aprender sobre Linux no Processo
==========
![ArchLinux2](https://github.com/ALTGNULinux/installarch/blob/master/src/ArchLinux2.png)

1 - Obtendo a ISO de instalação do Arch Linux
==========
Você pode obter a ISO de instalação do Arch na [pagina de download] (https://www.archlinux.org/download/). Como trata-se de uma distribuição Rolling Release, é gerada uma ISO atualizada mensalmente. Após obter a instalação, grave a ISO em um CD ou Pen Drive e inicie a instalação.

![ArchLinux2](https://github.com/ALTGNULinux/installarch/blob/master/src/1.1.png)

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

Inicie o Cfdisk:
>
	cfdisk
A interface de particionamento é essa:
![ArchLinux2](https://github.com/ALTGNULinux/installarch/blob/master/src/2.2.png)

***Em seguida particione o seu disco de acordo com oque você utilizar, GPT ou MBR.***

### MBR - BIOS
Sera criada a seguinte partição (onde X é o numero da partição):
>
	/dev/sdaX	 como /		com o tamanho que desejar e bootavel

No `cfdisk` devera ficar da seguinte forma:
![ArchLinux2](https://github.com/ALTGNULinux/installarch/blob/master/src/2.3.png)

Se tudo estiver certo grave as alterações no disco.
Agora iremos formatar e montar a partição criada:
>
	mkfs.ext4 /dev/sdaX  (A partição selecionada será formatada em ext4)
	mount /dev/sdaX /mnt (Será montada a partição em /mnt)
	mkdir /mnt/home (Sera criada a home dentro de /mnt)
