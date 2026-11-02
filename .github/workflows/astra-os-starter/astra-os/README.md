# Astra OS 1.0 (starter)

Primeira imagem Live personalizada do Astra OS, baseada em Debian 13 (Trixie),
para arquitetura amd64 (x86_64) e ambiente gráfico Xfce.

## O que esta primeira versão inclui

- Debian 13 (Trixie)
- Arquitetura amd64 (x86_64)
- Xfce (desktop)
- Chromium
- Thunar (gerenciador de arquivos)
- Terminal Xfce
- NetworkManager applet
- Ferramentas básicas
- Debian Installer em modo live, para permitir instalar o sistema no disco

## Importante

Esta é uma ISO Live de protótipo. Uma sessão Live iniciada pelo pendrive/ISO é
normalmente temporária: alterações podem ser perdidas ao desligar. Para um PC
Astra persistente, instale o sistema no disco virtual da VM e mantenha esse
disco entre desligamentos. Não exponha SSH, RDP ou VNC diretamente na internet.

## Como construir pelo GitHub (Windows + VS Code)

1. Crie um repositório GitHub vazio chamado `astra-os`.
2. Copie o conteúdo desta pasta para a raiz do repositório.
3. Faça commit e envie os arquivos para a branch `main`.
4. Abra a aba **Actions** do repositório.
5. Selecione **Build Astra OS** e clique em **Run workflow**.
6. Aguarde o workflow terminar com sucesso.
7. Abra a execução concluída e baixe o artefato `Astra-OS-1.0-ISO`.

O workflow usa um runner Ubuntu do GitHub com um contêiner Debian Trixie.
Assim, os comandos `apt-get`, `lb config` e `lb build` executam em Linux,
não no terminal PowerShell do Windows.

## Construir manualmente em Debian 13

Em uma instalação Debian 13 amd64, instale as ferramentas:

```sh
sudo apt update
sudo apt install live-build debootstrap cpio xorriso squashfs-tools \
  grub-pc-bin grub-efi-amd64-bin mtools dosfstools isolinux \
  syslinux-common rsync ca-certificates
```

Na raiz desta pasta, execute:

```sh
sudo chmod +x auto/config
sudo chmod +x config/hooks/normal/010-astra-branding.hook.chroot
sudo lb config
sudo lb build
```

O resultado esperado é `live-image-amd64.hybrid.iso`.

## Próxima etapa do Astra Cloud PC

Depois de testar a ISO, a próxima etapa é automatizar a instalação em um disco
virtual persistente e preparar o acesso remoto autenticado. A ISO, sozinha,
não cria nem hospeda máquinas virtuais para vários usuários.
