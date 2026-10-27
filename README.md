# W-OS

<p align="center">
  <img src="assets/wos-logo.png" alt="Logo do W-OS" width="420">
</p>

<p align="center"><strong>Um sistema operacional feito do zero para aprender como sistemas operacionais realmente funcionam.</strong></p>

## Geass

O **W-OS** é um sistema operacional x86 de 32 bits escrito do zero em **Assembly** e **C**. Ele não pretende competir com sistemas reais: é um laboratório educacional para explorar o que acontece entre ligar o computador e ter um sistema funcionando.

**Versão:** `0.8.3` · **Codinome:** `Geass`

---

## Um sistema que dá para enxergar por inteiro

Sem dependências externas e sem mistério: no W-OS, tudo o que acontece tem um motivo e deixa rastro.

- **Verificação de integridade nativa:** com `integrity init` e `integrity check`, o sistema registra uma linha de base dos arquivos e avisa quando algo muda, o mesmo princípio dos monitores de integridade usados em servidores reais.
- **Telemetria pela serial:** o kernel envia logs pela porta **COM1** e guarda mensagens no `dmesg`, então cada ação no shell pode ser acompanhada em tempo real do lado de fora da VM.
- **Superfície mínima:** sem driver de rede, sem processos de terceiros e sem bibliotecas externas. Tudo o que existe foi escrito do zero.
- **Boot em modo diagnóstico:** inicie sem GUI, sem mouse, sem disco, sem serial ou em modo seguro e observe como o sistema se comporta quando componentes são removidos.

---

## O que ele tem

- Bootloader próprio e entrada em modo protegido
- Kernel próprio com suporte a teclado e mouse PS/2
- Modo texto (VGA) e interface gráfica (VBE)
- Shell interativo com histórico de comandos
- Sistema de arquivos próprio (**WFS v2**) com persistência em disco
- Editor de texto integrado
- Auditoria e verificação de integridade
- Logs via serial COM1
- Menu de boot com modos de segurança e diagnóstico
- Integração experimental com o Doom

---

## Verifique a ISO antes de usar

Confira o hash antes de rodar:

```bash
sha256sum w-os.iso
```

Hash oficial da versão `0.8.3`:

```text
<COLE_AQUI_O_SHA256>
```

Se o valor for diferente, não use o arquivo.

---

## Como rodar

Você só precisa da ISO (`w-os.iso`) e de uma máquina virtual.

### VirtualBox

| Item | Valor |
|---|---|
| Tipo / Versão | Other / Other (32-bit) |
| Firmware | BIOS (sem EFI) |
| Memória | 128 MB ou mais |
| Armazenamento | Controlador IDE, com a ISO no CD virtual |
| Gráficos | VBoxVGA |
| Mouse | PS/2 |
| Rede | Desativada (ainda não há driver) |

### QEMU

```bash
qemu-system-i386 -cdrom w-os.iso -m 128
```

Para acompanhar os logs da serial direto no seu terminal:

```bash
qemu-system-i386 -cdrom w-os.iso -m 128 -serial stdio
```

---

## Menu de boot

```text
[Enter] modo normal
[2]     iniciar sem GUI
[3]     iniciar sem mouse
[4]     iniciar sem disco
[5]     iniciar sem serial
[9]     modo seguro, somente texto
```

---

## Experimentos para fazer

1. **Baseline e detecção:** rode `integrity init`, crie ou edite arquivos e use `integrity check` para ver o que foi detectado.
2. **Leitura de logs:** rode com `-serial stdio`, execute comandos no shell e relacione cada ação com o que aparece no log.
3. **Comparação de boots:** inicie em modo normal e em modo seguro e compare a saída de `dmesg`.
4. **Inventário do sistema:** use `cpu`, `free`, `disk` e `uname -a` para montar um perfil do ambiente.

---

## Comandos básicos

```text
help        lista os comandos disponíveis
ls          lista arquivos
cd          muda de pasta
cat         mostra o conteúdo de um arquivo
mkdir       cria uma pasta
edit        abre o editor de texto
history     mostra os últimos comandos
dmesg       mostra as mensagens do sistema
disk        informações do disco
cpu         informações do processador
free        uso de memória
uname -a    informações do sistema
tree        mostra a árvore de pastas
integrity   inicializa (init) e verifica (check) a integridade dos arquivos
doom        inicia o Doom (se houver pacote instalado)
```

Digite `help` para ver a lista completa.

---

## Limitações atuais

- Apenas x86 de 32 bits e boot via BIOS (sem UEFI)
- Sem driver de rede
- Sistema de arquivos sem journaling e com limites fixos
- Linha de comando de até 255 caracteres e histórico dos últimos 16 comandos
- O modo gráfico depende de suporte a VBE na máquina virtual

---

## Sobre o projeto

O W-OS nasceu de uma curiosidade simples: entender como um sistema operacional é construído de verdade, uma camada de cada vez, do bootloader ao shell.
