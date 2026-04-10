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
from rich.align import Align

console = Console()


def buscar_ip(ip):
    try:
        url = f"http://ip-api.com/json/{ip}?fields=status,message,country,regionName,city,zip,lat,lon,timezone,isp,org,query"
        resposta = requests.get(url, timeout=10)
        dados = resposta.json()

        if dados["status"] != "success":
            console.print(
                Panel(
                    f"[bold red]ERRO[/bold red]\n\n[bold red]{dados.get('message', 'IP inválido')}[/bold red]",
                    border_style="red",
                    title="[bold red]SYSTEM[/bold red]"
                )
            )
            return

        tabela = Table(
            title=f"[bold red]DADOS DO IP {dados['query']}[/bold red]",
            box=box.DOUBLE_EDGE,
            border_style="red",
            header_style="bold red"
        )

        tabela.add_column("CAMPO", style="red", no_wrap=True)
        tabela.add_column("INFORMAÇÃO", style="bold red")

        tabela.add_row("IP", dados["query"])
        tabela.add_row("PAÍS", dados["country"])
        tabela.add_row("REGIÃO", dados["regionName"])
        tabela.add_row("CIDADE", dados["city"])
        tabela.add_row("CEP", dados["zip"])
        tabela.add_row("LATITUDE", str(dados["lat"]))
        tabela.add_row("LONGITUDE", str(dados["lon"]))
        tabela.add_row("FUSO HORÁRIO", dados["timezone"])
        tabela.add_row("ISP", dados["isp"])
        tabela.add_row("ORGANIZAÇÃO", dados["org"])

        console.print()
        console.print(Align.center(tabela))
        console.print()

    except Exception as e:
        console.print(
            Panel(
                f"[bold red]ERRO AO BUSCAR IP[/bold red]\n\n[bold red]{e}[/bold red]",
                border_style="red"
            )
        )


while True:
    console.clear()

    logo = """
[bold red]
██    ██ ██████  ███    ██
██    ██ ██   ██ ████   ██
██    ██ ██████  ██ ██  ██
 ██  ██  ██      ██  ██ ██
  ████   ██      ██   ████
[/bold red]
"""

    painel_topo = Panel.fit(
        f"""{logo}
[bold red]VPN IP LOCATOR[/bold red]

[bold red]PAINEL DE LOCALIZAÇÃO DE IP[/bold red]

[bold red]@yrp369[/bold red]""",
        border_style="red",
        padding=(1, 5),
        title="[bold red]◢ CYBER SYSTEM ◣[/bold red]",
        subtitle="[bold red]STATUS: ONLINE[/bold red]"
    )

    console.print()
    console.print(Align.center(painel_topo))
    console.print()

    ip = Prompt.ask("[bold red]DIGITE O IP PARA LOCALIZAR[/bold red]")

    console.print()
    buscar_ip(ip)

    continuar = Prompt.ask(
        "\n[bold red]DESEJA PESQUISAR OUTRO IP? (s/n)[/bold red]",
        default="s"
    )

    if continuar.lower() != "s":
        console.print()
        console.print(
            Panel.fit(
                "[bold red]ENCERRANDO VPN SYSTEM...[/bold red]",
                border_style="red"
            )
        )
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
