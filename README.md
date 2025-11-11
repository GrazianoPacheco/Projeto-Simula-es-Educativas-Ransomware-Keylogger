Projeto: Simulações Educativas — Ransomware & Keylogger (apenas simulação)

Aviso importante: Este repositório contém simulações educativas. NUNCA execute scripts fora de um ambiente isolado (VM/sandbox) e NUNCA aponte os scripts para dados pessoais ou sistemas de produção. O conteúdo é para fins pedagógicos e de pesquisa defensiva.

Índice

Objetivo

Conteúdo do repositório

Requisitos e dependências

Como configurar o ambiente (passo a passo)

Como executar as simulações com segurança

Descrição dos componentes

Medidas de defesa e mitigação

Boas práticas e ética

Como documentar seu experimento / entregar o desafio

Licença

Objetivo

O objetivo deste projeto é aprender os mecanismos e sinais típicos de dois tipos de malware (ransomware e keylogger) apenas em ambiente controlado, estudando:

Como um ransomware pode operar (conceitualmente) e quais sinais deixaria em um sistema.

Como um keylogger funcionaria e como detectar/mitigar esse comportamento.

Técnicas de detecção (ex.: monitoramento de I/O, checagem de entropia, análise de comportamento).

Documentar resultados e recomendações de defesa.

Conteúdo do repositório

Estrutura sugerida (exemplo):

/my-malware-sim
├─ README.md
├─ LICENSE
├─ images/                     # capturas de tela sem dados sensíveis
├─ test_files/                 # arquivos de exemplo (dummy) para testes
├─ ransomware_sim/
│   ├─ encrypt_sim.py          # simulação segura: opera em cópias em test_files/
│   └─ decrypt_sim.py
├─ keylogger_sim/
│   └─ keylogger_mock.py       # mock/simulador que gera logs de exemplo (não captura teclado real)
├─ defensive_tools/
│   ├─ entropy_check.py        # ferramenta para medir entropia de arquivos
│   └─ write_activity_monitor.py
├─ requirements.txt
└─ report.md                   # relatório / explicações


Observação: os scripts *_sim.py e *_mock.py não devem capturar teclas reais nem criptografar arquivos reais do sistema. Eles devem trabalhar somente em test_files/ e/ou gerar arquivos de exemplo.

Requisitos e dependências

Recomendado:

Python 3.8+
Dependências (exemplo em requirements.txt):

cryptography        # apenas para simulação em cópias (opcional)
python-magic        # (opcional) identificação de tipos de arquivo


Use python -m pip install --user -r requirements.txt ou py -3 -m pip install --user -r requirements.txt se pip não estiver no PATH.

Como configurar o ambiente (passo a passo)
1) Crie uma VM isolada

Use VirtualBox, VMware, Hyper-V ou uma VM na nuvem (com cuidado). Faça snapshot antes de qualquer execução.

2) Clone o repositório dentro da VM
git clone <URL-DO-REPO>
cd <NOME-DO-REPO>

3) Crie ambiente virtual

Linux/macOS:

python3 -m venv venv
source venv/bin/activate
python -m pip install -r requirements.txt


Windows (PowerShell):

python -m venv venv
.\venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt


Se python não for reconhecido, use py -3 (Windows) ou instale Python.

Como executar as simulações com segurança

Preencha test_files/ com cópias de arquivos dummy (pequenos .txt, .jpg de teste). Não coloque documentos reais.

Execute apenas scripts marcados como simulação (ex.: encrypt_sim.py) e observe a saída.

Restaure snapshot da VM após os testes, se necessário.

Exemplo (Linux/Windows com venv ativo):

python ransomware_sim/encrypt_sim.py
python ransomware_sim/decrypt_sim.py
python keylogger_sim/keylogger_mock.py    # gera LOGS SIMULADOS, não captura teclado
python defensive_tools/entropy_check.py test_files/


Cada script deve exibir logs/textos explicando as ações (ex.: “Operando somente em test_files/ — gerando cópias encriptadas”).

Descrição dos componentes (resumo)
ransomware_sim/

encrypt_sim.py — simula o fluxo de um ransomware de forma segura: percorre test_files/, cria cópias nome.ext.enc (ou similar) contendo dados “simulados” ou criptografados usando uma chave local gerada para o teste. Não sobrescreve os originais. Mantém um backup e um log de operações.

decrypt_sim.py — reverte a operação nas cópias geradas pelo encrypt_sim.py.

keylogger_sim/

keylogger_mock.py — gera exemplos de linhas de log que imitam capturas de teclado (strings simuladas) para demonstrar análise de logs, sem rastrear teclas reais. Útil para treinar análise forense e classificação.

defensive_tools/

entropy_check.py — calcula entropia de arquivos para detectar aumento de entropia (possível indicativo de criptografia).

write_activity_monitor.py — exemplo de script que monitora criação/escrita de arquivos em um diretório e gera alertas se houver volume incomum.

Medidas de defesa e mitigação (resumo)

Backups offline e versionados: restauração rápida sem pagar resgate.

EDR / antivírus com heurística: detectar padrões de comportamento (massiva escrita, criação de extensões desconhecidas).

Monitoramento de integridade (file integrity monitoring).

Políticas de prvilégios mínimos: limitar contas que possam gravar em diretórios sensíveis.

Segmentação de rede e filtragem de tráfego de comando/controle.

Treinamento de usuários: identificar phishing (vetor comum).

No relatório (report.md) inclua testes que mostrem como o defensive_tools responde a cenas simuladas.

Boas práticas e ética

Nunca use este material para atacar terceiros.

Execute apenas com consentimento explícito e em ambiente controlado.

Documente claramente o propósito educacional e as limitações.

Se for publicar no GitHub, coloque um aviso no topo do README (como neste arquivo) e inclua um LICENSE apropriado (ex.: MIT).

Como documentar seu experimento / entregar o desafio

No report.md ou report.pdf inclua:

Objetivo do teste.

Ambiente (versão do SO, VM, snapshot).

Passos executados (com comandos).

Resultados observados (logs, prints, imagens).

Análise de defesa: como detectar, o que melhorar.

Conclusões e recomendações.

Adicione capturas de tela na pasta images/ com legendas claras.

Exemplos de mensagens que devem constar no repo

Aviso no topo: “Este repositório é para fins educacionais. Não execute em ambientes de produção.”

Instruções de rollback: “Faça snapshot antes e restaure após os testes.”

Licença

Escolha uma licença adequada. Para projetos educacionais, MIT ou CC BY-NC-SA (se quiser restringir uso comercial) são comuns.
