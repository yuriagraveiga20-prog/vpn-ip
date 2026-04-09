# vpn-ip # Painel Localizador de IP - VPN
projeto 09x3
## 1. Instale as dependências

```bash
sudo apt update
sudo apt install python3 python3-pip python3-venv -y
pip install requests rich
```

---

## 2. Crie a pasta do projeto

```bash
mkdir -p ~/vpn-ip-panel
cd ~/vpn-ip-panel
```

---

## 3. Crie o arquivo sem usar nano

Use este comando:

```bash
cat > painel_vpn.py << 'EOF'
import requests
from rich.console import Console
from rich.panel import Panel
from rich.table import Table
from rich.prompt import Prompt
from rich import box

console = Console()


def buscar_ip(ip):
    try:
        url = f"http://ip-api.com/json/{ip}?fields=status,message,country,regionName,city,zip,lat,lon,timezone,isp,org,query"
        resposta = requests.get(url, timeout=10)
        dados = resposta.json()

        if dados["status"] != "success":
            console.print(f"[red]Erro:[/red] {dados.get('message', 'IP inválido')}")
            return

        tabela = Table(title=f"Informações do IP {dados['query']}", box=box.DOUBLE_EDGE)
        tabela.add_column("Campo", style="cyan", no_wrap=True)
        tabela.add_column("Informação", style="green")

        tabela.add_row("IP", dados["query"])
        tabela.add_row("País", dados["country"])
        tabela.add_row("Região", dados["regionName"])
        tabela.add_row("Cidade", dados["city"])
        tabela.add_row("CEP", dados["zip"])
        tabela.add_row("Latitude", str(dados["lat"]))
        tabela.add_row("Longitude", str(dados["lon"]))
        tabela.add_row("Fuso Horário", dados["timezone"])
        tabela.add_row("ISP", dados["isp"])
        tabela.add_row("Organização", dados["org"])

        console.print(tabela)

    except Exception as e:
        console.print(f"[red]Erro ao buscar IP:[/red] {e}")


while True:
    console.clear()

    console.print(
        Panel.fit(
            "[bold green]VPN IP LOCATOR[/bold green]\n[white]Painel de Localização de IP[/white]",
            border_style="bright_blue"
        )
    )

    ip = Prompt.ask("Digite o IP para localizar")
    buscar_ip(ip)

    continuar = Prompt.ask("\nDeseja pesquisar outro IP? (s/n)", default="s")

    if continuar.lower() != "s":
        console.print("[bold red]Encerrando painel VPN...[/bold red]")
        break
EOF
```

---

## 4. Execute o painel

```bash
python3 painel_vpn.py
```

---

## 5. Resultado

O painel vai mostrar:

* País
* Região
* Cidade
* CEP
* Latitude
* Longitude
* ISP
* Organização
* Fuso horário

Ele também permanece aberto após a pesquisa, sem fechar sozinho. 🚀
