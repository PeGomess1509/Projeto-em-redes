# Projeto-em-redes

Este projeto demonstra a configuração e aplicação de protocolos de redundância HSRP (Hot Standby Router Protocol) e STP (Spanning Tree Protocol) em um ambiente de rede simulado, utilizando a topologia apresentada. O objetivo principal é garantir a alta disponibilidade e a resiliência da infraestrutura, minimizando o tempo de inatividade e otimizando o tráfego de rede.

Visão Geral do Projeto
Neste projeto, foi desenvolvida uma solução de rede robusta que incorpora mecanismos de redundância para evitar pontos únicos de falha. A arquitetura foi projetada para suportar interrupções em dispositivos de rede e links de comunicação, garantindo que os serviços críticos permaneçam acessíveis. A implementação focou na interconexão de múltiplos dispositivos de rede (roteadores e switches) para criar um ambiente tolerante a falhas, com destaque para a conectividade dos PCs (PC10-PC15) e Servidores (DHCP, DNS, WFR).

Protocolos de Redundância Aplicados
HSRP (Hot Standby Router Protocol)
O HSRP foi fundamental para garantir a redundância do gateway padrão para os hosts na rede. Na topologia, os roteadores Provedor A (2911) e Provedor B (2911) foram configurados em um grupo HSRP para fornecer um gateway redundante para os Switches Core (CORE 1 e CORE 2) e, consequentemente, para o restante da rede interna. Isso assegura que, em caso de falha de um dos roteadores de borda, o outro assuma automaticamente, mantendo a conectividade com a Internet e entre as redes internas.

Configurações Chave de HSRP Implementadas:

Prioridade: Definição de prioridades para determinar qual roteador de borda se tornaria o ativo.

Preempção: Habilitação da preempção para permitir que um roteador de maior prioridade reassuma a função de ativo quando retornar ao serviço.

Endereço IP Virtual: Configuração de um endereço IP virtual comum que os switches core utilizam como gateway padrão para acesso externo.

STP (Spanning Tree Protocol)
O STP foi essencial para prevenir loops de comutação na camada 2, que poderiam causar broadcasts infinitos e tornar a rede inoperante. Na topologia, existem diversos links redundantes entre os Switches Core (CORE 1 e CORE 2) e entre eles e os Switches de Acesso (C1, A12, DC). O STP foi configurado para bloquear portas redundantes, criando uma topologia lógica livre de loops, enquanto ainda permite caminhos alternativos em caso de falha de um link ou switch, garantindo que o tráfego dos PCs (192.168.10.x, 192.168.20.x) e Servidores (172.16.9.x) continue fluindo.

Configurações Chave de STP Implementadas:

Root Bridge: Designação de um switch raiz (provavelmente um dos switches CORE) para otimizar o fluxo de tráfego na camada 2.

Prioridades de Porta/VLAN: Ajustes nas prioridades em VLANs específicas (por exemplo, para as VLANs dos PCs e Servidores) para influenciar o caminho de forwarding preferencial e aprimorar o balanceamento de carga ou a resiliência.

PortFast/BPDU Guard: Aplicação de recursos como PortFast em portas de acesso (conectadas aos PCs e Servidores) para aceleração da transição de estado da porta, e BPDU Guard para segurança, prevenindo a injeção de BPDUs maliciosas.

Topologia da Rede
Descrição da Topologia: A rede é composta por dois roteadores de borda (Provedor A e B) que fornecem conectividade à Internet e redundância via HSRP. Eles se conectam a dois switches core (CORE 1 e CORE 2), que formam a espinha dorsal da rede e também estão interligados de forma redundante. Três switches de acesso (C1, A12, DC) se conectam aos switches core, fornecendo conectividade para diferentes segmentos da rede:
* Switch C1: Conecta PCs nas VLANs 10 (192.168.10.x) e 12 (192.168.20.x).
* Switch A12: Conecta PCs nas VLANs 13, 14 e 15 (192.168.20.x).
* Switch DC: Conecta os servidores DHCP, DNS e WFR (172.16.9.x).
Os links redundantes entre os switches garantem resiliência na camada 2, gerenciados pelo STP para evitar loops.
