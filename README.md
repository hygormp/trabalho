⚙️ Preparação do ambiente
1. Verificar o sistema operacional
No terminal do Kali Linux, execute:


cat /etc/os-release
2. Verificar a configuração de rede

ip addr
3. Verificar o Git

git --version
4. Instalar o Medusa
Se a ferramenta ainda não estiver instalada:


sudo apt update
sudo apt install medusa git
5. Consultar a ajuda do Medusa

medusa -h
Para consultar os módulos disponíveis:


medusa -d
Para consultar a documentação local:


man medusa
Os parâmetros disponíveis podem variar conforme a versão instalada. Consulte a documentação local antes de preparar qualquer teste.

6. Verificar a conectividade do laboratório
Substitua o endereço abaixo pelo IP real do servidor de teste:


ping -c 3 192.168.56.20
A ausência de resposta não significa necessariamente que o servidor esteja indisponível, pois o tráfego ICMP pode estar bloqueado.

🔎 Metodologia de auditoria
O projeto será desenvolvido em cinco fases.

Fase 1 — Preparação e inventário
Identificar os ativos autorizados.
Registrar endereços IP e serviços.
Documentar versões e configurações relevantes.
Criar contas fictícias.
Definir limites de execução e critérios de interrupção.
Fase 2 — Avaliação inicial
Examinar as políticas de autenticação.
Verificar a proteção das credenciais durante a transmissão.
Avaliar mecanismos de limitação de tentativas.
Identificar recursos de monitoramento e registro.
Registrar o comportamento normal dos serviços.
Fase 3 — Testes controlados
Realizar poucas tentativas inválidas com contas de laboratório.
Utilizar o Medusa somente dentro do escopo autorizado.
Evitar grandes listas de senhas e volumes elevados de tentativas.
Observar o comportamento dos serviços.
Registrar os horários dos testes para facilitar a análise dos logs.
Fase 4 — Análise dos resultados
Examinar os eventos de autenticação.
Verificar se as falhas foram registradas.
Avaliar a presença de limitação de tentativas.
Documentar os controles ausentes ou inadequados.
Associar cada conclusão a uma evidência.
Fase 5 — Mitigação e reteste
Aplicar as correções necessárias.
Repetir os testes controlados.
Comparar o comportamento antes e depois das correções.
Registrar os resultados do reteste.
Atualizar os relatórios e a matriz de riscos.
📂 Auditoria por serviço
1. FTP — File Transfer Protocol
Objetivo: avaliar a segurança da autenticação e a proteção das credenciais no serviço FTP.

Verificações
Identificar a versão do serviço.
Verificar o método de autenticação.
Avaliar se as credenciais podem ser transmitidas sem criptografia.
Realizar tentativas inválidas limitadas com uma conta fictícia.
Analisar os logs de autenticação.
Verificar restrições de acesso e controles de tentativas.
Validar as medidas de mitigação.
Vulnerabilidades potenciais
Senhas fracas ou reutilizadas.
Transmissão de credenciais sem criptografia.
Ausência de limitação de tentativas.
Permissões excessivas em contas.
Exposição desnecessária do serviço à rede.
Medidas de mitigação
Preferir SFTP ou FTPS adequadamente configurado em vez de FTP sem criptografia.
Aplicar políticas de senhas fortes e exclusivas.
Restringir o acesso por rede e firewall.
Monitorar falhas repetidas de autenticação.
Aplicar o princípio do menor privilégio.
Evidências
Configuração do serviço.
Registro de tentativas inválidas.
Evidência do mecanismo de proteção de transporte.
Resultado da validação após as correções.
2. Web — Autenticação de aplicações web
Objetivo: avaliar a proteção de formulários de login contra tentativas repetidas de autenticação.

Verificações
Identificar a rota de autenticação da aplicação de laboratório.
Verificar a utilização de HTTPS quando aplicável.
Realizar tentativas inválidas limitadas com uma conta fictícia.
Examinar as respostas da aplicação.
Analisar os logs do servidor e da aplicação.
Verificar mecanismos de limitação de tentativas.
Avaliar se as respostas revelam informações excessivas sobre usuários.
Vulnerabilidades potenciais
Ausência de limitação de tentativas.
Senhas fracas.
Falta de autenticação multifator quando apropriada.
Monitoramento insuficiente.
Mensagens que revelam a existência de contas.
Falta de controles contra automação abusiva.
Medidas de mitigação
Implementar rate limiting.
Aplicar atrasos progressivos conforme o risco.
Utilizar autenticação multifator quando apropriada.
Monitorar tentativas anormais.
Utilizar mecanismos contra automação abusiva.
Evitar mensagens de erro que revelem informações desnecessárias.
Garantir o uso de HTTPS e uma gestão segura das sessões.
Evidências
Capturas de tela da aplicação de teste.
Registros de autenticação.
Evidências dos controles aplicados.
Comparação dos resultados antes e depois da mitigação.
3. SMB — Server Message Block
Objetivo: avaliar a segurança da autenticação e das permissões de compartilhamento de arquivos.

Verificações
Identificar a versão do servidor SMB.
Verificar se SMBv1 está desativado.
Criar uma conta de teste com permissões mínimas.
Realizar tentativas inválidas limitadas.
Examinar os eventos de autenticação.
Verificar as permissões dos compartilhamentos.
Avaliar as restrições de acesso por rede.
Repetir os testes após as correções.
Vulnerabilidades potenciais
Senhas fracas.
Uso de protocolos antigos.
Compartilhamentos excessivamente permissivos.
Exposição desnecessária das portas do serviço.
Privilégios excessivos.
Monitoramento inadequado de tentativas de autenticação.
Medidas de mitigação
Desativar SMBv1.
Manter o servidor atualizado.
Aplicar políticas de senhas adequadas.
Restringir o acesso por firewall e segmentação de rede.
Revisar permissões de usuários e compartilhamentos.
Utilizar o menor privilégio necessário.
Monitorar eventos de autenticação.
Evidências
Configuração do serviço SMB.
Registros de falhas de autenticação.
Permissões dos compartilhamentos.
Evidências de restrição de acesso.
Resultados dos testes após a mitigação.
📊 Matriz de avaliação
Os itens abaixo representam verificações planejadas. Nenhuma vulnerabilidade deve ser considerada confirmada antes da execução e análise dos testes.


ID	Verificação	Critério de aprovação
FTP-01	Proteção das credenciais	Transporte criptografado e configuração adequada
FTP-02	Tentativas inválidas	Registro de eventos e controles de tentativas documentados
WEB-01	Limitação de tentativas	Política eficaz e testada
WEB-02	Monitoramento	Eventos anormais identificáveis nos logs
SMB-01	Protocolo SMB	SMBv1 desativado
SMB-02	Permissões	Acesso restrito e privilégios mínimos
GEN-01	Documentação	Evidências associadas aos testes
GEN-02	Reteste	Correções validadas com novas evidências
Classificação dos resultados
Aprovado: o critério foi verificado e atendido.
Reprovado: foi observada uma falha em relação ao critério.
Parcial: parte do critério foi atendida, mas existem limitações.
Não verificado: não há evidências suficientes para uma conclusão.
A ausência de falhas durante um teste limitado não comprova, isoladamente, que o serviço está protegido contra todos os ataques de força bruta.

📝 Modelo de relatório técnico
Para cada serviço, registre os seguintes dados:

Identificação
Serviço:
Versão:
Endereço do alvo de laboratório:
Data e hora:
Responsável:
Identificador do teste:
Objetivo
Descrever o controle de segurança que será avaliado.

Procedimento
Registrar as etapas executadas, as configurações verificadas e os limites definidos.

Resultados
Quantidade de tentativas realizadas:
Falhas registradas:
Controles observados:
Problemas identificados:
Resultado após a mitigação:
Evidências
Identificador da evidência:
Nome do arquivo:
Data e hora:
Descrição:
Dados sensíveis removidos:
Avaliação de risco
Status do teste:
Impacto potencial:
Probabilidade estimada:
Justificativa:
Limitações da avaliação:
Recomendações
Descrever as correções propostas e o método de validação de cada uma.

Conclusão
Apresentar apenas conclusões sustentadas por configurações verificadas e evidências coletadas.

📸 Evidências seguras
As evidências devem demonstrar o procedimento e os resultados sem expor dados sensíveis.

Tipos de evidências
Capturas de tela da arquitetura do laboratório.
Configurações relevantes dos serviços.
Logs de falhas de autenticação.
Registros dos controles de segurança.
Comparação entre os resultados anteriores e posteriores à mitigação.
Convenção de nomes

EVD-001-rede-laboratorio.png
EVD-002-configuracao-ftp.png
EVD-003-falhas-autenticacao.png
EVD-004-controle-aplicado.png
EVD-005-reteste.png
Cada evidência deve possuir uma descrição no relatório e um identificador único.

Integridade dos arquivos
É possível calcular o hash SHA-256 de uma evidência sanitizada:


sha256sum EVD-003-falhas-autenticacao.png
O hash pode ajudar a identificar alterações posteriores no arquivo, mas não comprova sozinho a autenticidade ou a origem da evidência.

Cuidados antes da publicação
Remover senhas, tokens e chaves privadas.
Não publicar credenciais reais.
Remover informações pessoais e dados internos sensíveis.
Revisar os arquivos antes de adicioná-los ao Git.
Verificar o histórico de commits antes de tornar o repositório público.
🛡️ Plano de mitigação

Risco	Medida de mitigação	Método de validação
Senhas fracas	Políticas de senha e bloqueio de senhas comprometidas	Revisar a política e testar contas fictícias
Tentativas repetidas	Rate limiting e atrasos progressivos	Verificar o comportamento durante testes controlados
Credenciais sem criptografia	Preferir SFTP ou FTPS para transferência de arquivos	Verificar a configuração do transporte
Login Web desprotegido	Limitação de tentativas e MFA quando apropriada	Validar os controles e os logs
SMB exposto	Restrição de acesso por rede	Verificar a acessibilidade autorizada
Protocolo SMB antigo	Desativar SMBv1 e atualizar o servidor	Confirmar a configuração efetiva
Falta de monitoramento	Centralização de logs e alertas	Confirmar a geração e o recebimento dos eventos
O bloqueio permanente de contas pode ser explorado para causar negação de serviço contra usuários legítimos. Os controles devem ser proporcionais ao risco e incluir mecanismos seguros de recuperação de acesso.

📁 Estrutura do repositório

auditoria-autenticacao-lab/
├── README.md
├── LICENSE
├── .gitignore
├── docs/
│   ├── escopo.md
│   ├── arquitetura.md
│   ├── metodologia.md
│   └── matriz-riscos.md
├── resultados/
│   ├── ftp.md
│   ├── web.md
│   └── smb.md
├── evidencias/
│   └── README.md
└── mitigacoes/
    └── plano-correcao.md
O README apresenta o projeto. Os relatórios individuais devem conter os detalhes específicos de cada serviço.

☁️ Publicação no GitHub
1. Inicializar o repositório local
Na pasta do projeto, execute:


git init
2. Verificar os arquivos

git status
3. Adicionar os arquivos revisados

git add README.md docs resultados mitigacoes evidencias
4. Revisar o conteúdo preparado para o commit

git diff --cached
5. Criar o commit

git commit -m "docs: adiciona projeto de auditoria de autenticacao"
6. Configurar a branch principal

git branch -M main
7. Associar ao repositório remoto
Depois de criar um repositório vazio no GitHub, substitua o endereço abaixo pela URL real do seu repositório:


git remote add origin <URL-DO-SEU-REPOSITORIO>
git push -u origin main
Antes da publicação, verifique se nenhum arquivo contém informações sensíveis. Remover um arquivo do diretório atual não remove necessariamente seus dados de commits anteriores.

📅 Cronograma sugerido
Semana 1 — Preparação
Configurar as máquinas virtuais.
Isolar a rede do laboratório.
Instalar e verificar as ferramentas.
Documentar o escopo.
Semana 2 — Auditoria
Estudar os módulos do Medusa.
Avaliar FTP, Web e SMB.
Coletar logs e evidências.
Registrar os resultados iniciais.
Semana 3 — Mitigação
Aplicar as correções.
Validar os controles de segurança.
Repetir os testes limitados.
Documentar as diferenças observadas.
Semana 4 — Portfólio
Finalizar os relatórios.
Revisar as evidências.
Organizar o repositório.
Publicar a documentação no GitHub.
✅ Checklist de conclusão
Configurei uma rede virtual isolada.
Verifiquei o Kali Linux, Git e Medusa.
Documentei o escopo e os ativos autorizados.
Avaliei os serviços FTP, Web e SMB.
Coletei logs e evidências sanitizadas.
Documentei os riscos e as limitações.
Registrei as medidas de mitigação.
Validei as correções com retestes.
Revisei os arquivos e o histórico do Git.
Publiquei a documentação no GitHub.
📚 Aprendizados esperados
Ao concluir este projeto, espero desenvolver conhecimentos e habilidades relacionados a:

Fundamentos de segurança da informação.
Ataques de força bruta e de dicionário.
Autenticação em serviços de rede.
Utilização básica do Kali Linux e Medusa.
Análise de logs e eventos de segurança.
Identificação e mitigação de vulnerabilidades.
Avaliação de riscos.
Documentação técnica.
Controle de versão com Git.
Construção de portfólio profissional no GitHub.
⚠️ Aviso legal e ético
Este projeto possui finalidade educacional e de defesa cibernética.

Todas as atividades devem ser realizadas em ambientes próprios, isolados ou expressamente autorizados. Não execute testes de autenticação contra sistemas de terceiros sem autorização.

As evidências devem ser sanitizadas antes da publicação, e os resultados precisam refletir fielmente os procedimentos realizados.

👤 Autor
Nome: [Seu nome]

GitHub: [Link para seu perfil]

Área de interesse: Segurança da Informação e Cibersegurança

🏁 Conclusão
Este projeto apresenta um processo estruturado de auditoria de autenticação para os serviços FTP, Web e SMB, combinando testes controlados, análise de logs, avaliação de riscos e medidas de mitigação.

A documentação no GitHub permitirá acompanhar a metodologia, revisar as evidências e compreender as decisões técnicas adotadas.

Os resultados finais serão preenchidos após a execução real dos testes no laboratório, garantindo transparência e credibilidade ao portfólio.
