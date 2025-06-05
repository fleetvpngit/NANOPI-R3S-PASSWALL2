# Instalação do Passwall2 no OpenWRT - NanoPi R3S

Certifique do aparelho estar conectado em alguma internet para baixar os pacotes necessários.

### 🔻 1. Atualizar lista de pacotes e substituir o dnsmasq
```sh
opkg update
opkg remove dnsmasq
opkg install dnsmasq-full
```
### 🔻 2. Instalar módulos necessários para TPROXY
```sh
opkg install kmod-nft-tproxy kmod-nft-socket
opkg install ipset
opkg install ipt2socks
opkg install iptables
opkg install iptables-legacy
opkg install iptables-mod-conntrack-extra
opkg install iptables-mod-iprange
opkg install iptables-mod-socket
opkg install iptables-mod-tproxy
opkg install kmod-ipt-nat
opkg install kmod-tun
```

### 🔻 3.  Adicionar a chave pública do repositório Passwall
```sh
wget -O passwall.pub https://master.dl.sourceforge.net/project/openwrt-passwall-build/passwall.pub
opkg-key add passwall.pub
```

### 🔻 4.  Configurar os feeds personalizados do Passwall
Execute o script abaixo para identificar sua versão e arquitetura do OpenWRT, e adicionar os repositórios ao arquivo customfeeds.conf:
```sh
release=$( . /etc/openwrt_release; echo ${DISTRIB_RELEASE%.*} )
arch=$( . /etc/openwrt_release; echo $DISTRIB_ARCH )
for feed in passwall_packages passwall2; do
  echo "src/gz $feed https://master.dl.sourceforge.net/project/openwrt-passwall-build/releases/packages-${release}/${arch}/${feed}" >> /etc/opkg/customfeeds.conf
done
```

### 🔻 5.  Atualizar os feeds e instalar o Passwall2
```sh
opkg update
opkg install luci-app-passwall2
opkg update
```

Após a instalação
O Passwall2 estará disponível no LuCI (interface web do OpenWRT) para configuração.

Certifique-se de reiniciar o sistema se necessário.