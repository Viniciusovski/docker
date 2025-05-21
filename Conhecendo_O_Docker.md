# docker


💡 Cenário Inicial
Temos três aplicações que precisam funcionar ao mesmo tempo:

Aplicação em C# (.NET 9) → Porta 80

Aplicação em Java (Java 17) → Porta 80

Nginx (1.17.0) → Porta 80

🔧 Todas estão tentando usar a mesma porta 80, o que causa conflito porque uma porta só pode ser usada por uma aplicação por vez.

❓ Desafios do cenário
Conflito de portas

Gerenciamento de versões (C#, Java, Nginx)

Controle de recursos (CPU e memória)

Manutenção e escalabilidade a longo prazo

🛠️ Soluções possíveis
✅ Solução simples, mas cara:
Ter uma máquina separada para cada aplicação

Resolve conflitos e facilita o controle de recursos

Problema: Custo altíssimo e difícil de escalar

🖥️ Solução tradicional:
Máquinas Virtuais (VMs)

Isolam as aplicações com seus próprios sistemas operacionais

Uso de hypervisor para criar sistemas virtuais dentro de um host físico

Vantagem: Isolamento total

Desvantagem: Consomem muitos recursos e são mais pesadas

🧱 Solução moderna e leve: Containers
Não usam um sistema operacional completo como nas VMs

Funcionam como processos isolados dentro do mesmo SO

São mais leves, rápidos e eficientes

🔍 Como os containers conseguem isso?
1. Isolamento com Namespaces
PID: isola processos

NET: isola rede

IPC: isola comunicação entre processos

MNT: isola sistema de arquivos

UTS: isola e compartilha informações do kernel

Graças ao namespace UTS, o container não precisa instalar um sistema operacional completo, pois compartilha o kernel do host, com isolamento.

2. Controle de recursos com Cgroups
Permite limitar uso de CPU, memória, etc.

Cada container pode ter um perfil de consumo definido

✅ Resumo Final
Containers resolvem problemas de portas, versões, isolamento e consumo de recursos

São mais eficientes que máquinas virtuais

Utilizam o kernel do sistema original, com isolamento garantido por namespaces

Recursos são gerenciados com Cgroups

Ideal para escalar, manter e distribuir aplicações modernas

