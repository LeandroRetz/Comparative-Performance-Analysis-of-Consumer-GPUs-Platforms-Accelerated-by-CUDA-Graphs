# Resumo (Abstract)
Este trabalho investiga a viabilidade de reproduzir benchmarks originalmente executados em GPUs de datacenter (NVIDIA A100 e RTX 8000) utilizando placas de vídeo de consumo (NVIDIA GeForce RTX 3050 e GTX 1060) com suporte a CUDA Graphs.

Algoritmos selecionados (BT, LU, SP, EP, IS, MG e CG) foram avaliados em diferentes classes de problemas. O estudo foca no tempo de execução como métrica principal, comparando execuções com e sem CUDA Graphs para quantificar ganhos de desempenho. Resultados mostram que, enquanto as GPUs empresariais superam em cargas de trabalho limitadas por memória, o hardware de consumo moderno permite a reprodução econômica de experimentos científicos moderados, apoiando a democratização da pesquisa em computação de alto desempenho (HPC).

---

# Ambiente Experimental & Reprodutibilidade
Estes passos detalham a configuração no **Ubuntu 24.04 LTS**, incluindo a resolução manual de dependências legadas exigidas pelo **CUDA 12.4**.

### 1. Preparação do Sistema & Dependências Legadas
Instale o essencial de compilação e a `libtinfo5` (descontinuada no Ubuntu 24.04, mas necessária para o instalador CUDA):

```bash
sudo apt update
sudo apt install -y git build-essential gcc g++ wget
wget [http://security.ubuntu.com/ubuntu/pool/universe/n/ncurses/libtinfo5_6.3-2ubuntu0.1_amd64.deb](http://security.ubuntu.com/ubuntu/pool/universe/n/ncurses/libtinfo5_6.3-2ubuntu0.1_amd64.deb)
sudo apt install ./libtinfo5_6.3-2ubuntu0.1_amd64.deb -y
### 2. Instalação do CUDA Toolkit 12.4
Utilizando o repositório do NVIDIA Ubuntu 22.04 para compatibilidade:

Bash
wget [https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2204/x86_64/cuda-keyring_1.1-1_all.deb](https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2204/x86_64/cuda-keyring_1.1-1_all.deb)
sudo dpkg -i cuda-keyring_1.1-1_all.deb
sudo bash -c 'echo "deb [signed-by=/usr/share/keyrings/cuda-archive-keyring.gpg] [https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2204/x86_64/](https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2204/x86_64/) /" > /etc/apt/sources.list.d/cuda-ubuntu2204-x86_64.list'
sudo apt update
sudo apt install -y cuda-toolkit-12-4 cuda-drivers
sudo reboot
### 3. Configuração de Caminhos (Path)

Bash
sudo ln -sfn /usr/local/cuda-12.4 /usr/local/cuda
echo 'export PATH=/usr/local/cuda/bin:$PATH' >> ~/.bashrc
echo 'export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH' >> ~/.bashrc
source ~/.bashrc
# Fluxo de Execução

### Compilação & Otimização de Arquitetura
Para obter o desempenho máximo, compile a suíte NPB especificamente para a Compute Capability (CC) da sua GPU:

Identificar CC: nvidia-smi --query-gpu=compute_cap --format=csv

Editar config/make.def: Ajuste a flag COMPUTE_CAPABILITY.

RTX 3050 (CC 8.6): -gencode arch=compute_86,code=sm_86

GTX 1060 (CC 6.1): -gencode arch=compute_61,code=sm_61

Exemplo: Compilar Block Tridiagonal (BT) Classe C

Bash
make bt CLASS=C
### Protocolo Experimental
Para confiabilidade estatística conforme descrito na Metodologia:

Total de Execuções: 13 execuções consecutivas por configuração.

Warm-up: Descarte as 3 primeiras execuções.

Dados: Média das 10 execuções válidas restantes.

Bash
cd bin
./bt.C
# Visão Geral dos Resultados

Desempenho Estável: A RTX 3050 entrega comportamento consistente, tipicamente 6x–12x mais lenta que a A100, mas significativamente mais rápida que a GTX 1060.

Limites de Memória: A GTX 1060 (3GB VRAM) enfrenta limitações severas em benchmarks de Classe C devido aos requisitos de alocação de grafos.

Gargalo de Banda: Aplicações com intenso throughput de memória (benchmark IS) permanecem altamente dependentes da memória HBM2 das GPUs enterprise.
