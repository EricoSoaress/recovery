Linha do Tempo MESTRE — Ataque POP-SERVER-02 (Pop Clínica)

Projeto AUDITORIA · consolida todos os docs e relatórios de sessão do projeto · atualização: 04/10/2026 (v3)

Fontes: .evtx do servidor, hives, USN, cache/logs RDP e histórico do Chrome/Edge da MOM-ESC-TI-01, metadados BitLocker, conta Microsoft do Flávio, Windows.edb. Horários em BRT (UTC−3). O relógio do servidor está ~20 s adiantado em relação à MOM; no minuto, as fontes se alinham. Marcadores: [SRV] POP-SERVER-02 · [MOM] máquina do atacante (MOMEACAI\tecnologia / perfil maikon.doglas) · [CHROME] navegador da MOM.

Mapa de discos (estação de perícia pc-erico): C: = disco do pc-erico · D: = antigo C: do servidor (sistema, legível) · E: = antigo D: do servidor (dados, bloqueado por BitLocker). O disco original foi preservado; trabalha-se sobre cópias/imagens.

1. Contexto pré-ataque
Quando	Evento
30/07/2025	[SRV] DB Signature do Windows.edb — marco antigo (referência de custódia)
05/08/2026 21:18	[SRV] BitLocker/Device Encryption legítimo no volume C: (XTS-AES 128, GUID {94d28ed9…}) — não confundir com o ataque
(config)	[SRV] Servidor em WORKGROUP (sem AD no momento do ataque; coluna Domínio = nome da máquina) → sem escrow automático possível
desde ~28/09	[SRV] força bruta de credenciais (milhares de 4625) de IPs externos (62.164.177.27/.28, 88.214.25.123/.125, 112.170.79.190) e internos (fe80::, 192.168.1.110) → movimento lateral
2. Dia do ataque — 30/09/2026 (linha unificada)
Hora (BRT)	Fonte	Evento
15:57–16:19	[MOM]	Tentativas RDP para 192.168.88.62 (LAN interna), falham (516)
16:08–16:12	[SRV]	RDP com sucesso como ana.pop / ana.xavier de 181.224.86.101 (acesso legítimo da Ana — não confundir)
16:21:44–16:22:04	[MOM]	Conecta em 177.54.238.76:33389 → destino se identifica como POP-SERVER-02 (sessão 476); usuário digitado = "call center 03" (prova por hash 1029 pvx9…)
16:22:23	[SRV]	Logon RDP Call Center 03 (sessão 476) de 181.224.84.98 (= MOM-ESC-TI-01)
16:22–16:23	[CHROME]	ChatGPT: "cria um .bat para apagar todos os arquivos" / "Limpeza segura do Windows"
16:25	[CHROME]	Gemini recusa modificação destrutiva
16:49–16:57	[SRV]	Cadeia de elevações UAC (consent.exe ::1) da Administracao (~8 min)
16:55	[CHROME]	Google: "onde fica o ponto de restauração do windows 10"
16:55:36	[SRV]	Cópia de sombra montada (NTFS evt 98, HarddiskVolumeShadowCopy7)
16:57:10	[SRV]	"Redefinir este PC" (WIPE) disparado → falha 16:57:45 (0x80070539, SID …-1001.bak) → abortado 16:57:52 sem resetar (a máquina sobreviveu)
16:57:43	[MOM]	Desconexão RDP (código 1)
16:58:02 / 16:58:30	[MOM]/[SRV]	Reconecta como administracao (prova por hash 1029 c5Xz…) → sessão 477
17:07–17:08	[CHROME]	ChatGPT: "gere uma senha forte de 24 caracteres" → conversa "Gerar senha forte" (c/6abd6c2d…, conta chatgpt-mome@momeacai.com.br) = a senha do BitLocker
17:08:36–17:10:25	[SRV]	Reset de senha de 11 contas (Administracao, Administrador, TI, Suporte, Sintec, Wanessa ×3, Erico, Victor.Pires, renan.cruz)
17:11:26–17:17:14	[SRV]	Desabilita ~60 contas
17:19:09	[SRV]	Reinício manual (1074, explorer.exe)
17:21:06–08	[SRV]	Boot; PostgreSQL 11 sobe normal → banco íntegro no D: às 17:21
17:21:23	[MOM]	Reconecta → sessão 3
17:24:42–17:26:18	[SRV]	Desabilita +8 contas e apaga 10 contas de recepção
17:27–17:29	[SRV]	Logon "Pablo Ed Saude" de 38.224.188.14 (3º IP externo)
17:29:40	[SRV]	Abre services.msc
17:48	[SRV]	Instala CrystalDiskInfo via winget (7 min antes do BitLocker)
17:53	[CHROME]	Google: "comando para apagar disco c e d pelo powershell mesmo com programa rodando"
17:55:03	[SRV]	BitLocker ativado no volume de dados (evt 796, por software)
17:55:13	[SRV]	Protetor de SENHA (775, 0x8, F68473D0) — passphrase digitada
17:55:16	[SRV]	Protetor de RECUPERAÇÃO (775, 0x3, ID E7D36899-91DE-4834-B00A-9499D849ECB1)
17:55:34	[SRV]	Campo de identificação alterado (780) + criptografia iniciada (768, XTS-AES 128, "somente espaço usado")
17:55:20 / 17:55:37	[SRV]	TXT da chave gravado na Área de Trabalho (Administracao.POP-SERVER-02, local, não \\tsclient); indexador processa o arquivo (confirma existência)
18:08:17	[SRV]	Abre msconfig
18:22:01	[SRV]	Reinício manual (1074)
18:21:48 / 18:22:08	[MOM]/[SRV]	Sessão 3 encerrada (código 4) = a queda vista na MOM; conta tecnologia faz logoff 18:24
18:23:06–08	[SRV]	Servidor volta; D: bloqueado; PostgreSQL não inicia (7000)
18:25:28	[SRV]	mario.popclinica logon no console (físico)
18:52	[SRV]	mario.popclinica via RDP de 192.168.1.135 (ARN-NT-CD62ZM3)
após 19:30	[SRV]	Acessos da equipe do Erico
22:00–23:28	[SRV]	Loop de travamento do explorer.exe → USN de 32 MB recicla e perde parte da janela

Ordem do dolo: tentar zerar a máquina (16:57) → destruir contas (17:08–17:26) → trancar o volume de dados com BitLocker (17:55). Negação de acesso + extorsão.

3. Pós-ataque e descoberta — 01/10/2026
Quando	Evento
01/10 (madrugada)	[SRV] Conexões AnyDesk entrantes (Token 1404083879)
01/10 05:37	[SRV] Criado EMERGENCIA\bitlocker.txt + reinicios.txt = exports de eventos da resposta (não a chave; o bitlocker.txt é saída de manage-bde/eventos, inclusive de outra máquina ARN-NT-CD62ZM3)
01/10	Clínica descobre o ataque; servidor fora do ar
4. Linha do tempo da PERÍCIA (02–04/10/2026)
Quando	Ação	Resultado
02/10 ~17:11	Perícia inicial da MOM (EVIDENCIA_MOM): cache RDP, auditoria, hashes	Cache de bitmap coletado; achados = nome do arquivo
03/10 ~08:45	Coleta das provas primárias da MOM (ORIGINAIS): .evtx/.reg/.rdp com hash	Integridade conferida (sem 1102; EventRecordID contínuo)
03/10 ~09:46	Busca da chave no disco do servidor v1 (Buscar-ChaveBitLocker.ps1)	Inválida (bug -Include: extensão sem *) → falso negativo
03/10 ~19:00	Análise do Windows.edb (Varredura_chave_Windows_edb.ps1)	❌ só o NOME do TXT; sem os 48 dígitos. Lateral: PDFs no Edge BBFMA 07-2026.pdf, AMEX 06-2026.pdf
03/10 16:50	Cópia forense do disco do servidor (robocopy /B) → COPIA_DISCO_D; hives; USN; carving; entropia E:; wordlist	Inventário 293.103 arquivos, chave não existe no disco
04/10	Conta Microsoft do Flávio (flavio.oliveira@popclinica.com.br) checada	POP-SERVER-02 vinculado à MSA, mas a chave E7D36899 NÃO está lá (só 3 outras máquinas) → sem escrow
04/10	Confirmação manage-bde do E: (2.0, XTS-AES 128, 2 protetores) + log BitLocker	Sem evento 845/846/847 → nunca houve escrow (AD/MSA/nuvem)
04/10 07:39	Achado: contato de extorsão datastore@cyberfear	Extorsão direcionada
04/10 10:15–11:21	Aquisição da imagem da MOM (PhysicalDrive0.raw, SHA-256 95b9ae8f…b69a, 0 erros)	Máquina do atacante preservada
04/10	Carve imagem/pagefile/senha; cópia perfil maikon; coleta de rastros; histórico do Chrome	Pagefile/clipboard/Notepad/Timeline negativos; premeditação + conta ChatGPT descobertas
04/10	carve_extra_MOM (hiberfil + Sessions + dumps)	IndexedDB do ChatGPT: PROMPT sim, resposta nunca; demais negativos
04/10	Wordlist dirigida wordlist_MOM_adicoes.txt (55.160)	Pronta p/ hashcat -m 22100
04/10	Análise de exfiltração (AnyDesk/TeamViewer/downloads)	Sem exfiltração (só config SiTef)
04/10	USN do servidor + hiberfil da MOM (6,1 GB, snapshot 03/10)	bitlocker.txt=resposta; hiberfil negativo
04/10	Rescan do pagefile do servidor (9,1 GB, carve_v3_universal.ps1, bug do read curto corrigido → leu tudo)	❌ Negativo p/ chave e senha (RAM reciclada). Lateral: rastro .tmp.driveupload (Google Drive) em Documentos Compartilhados
04/10	Inventário + esta linha do tempo mestre + relatórios de sessão	Cadeia de custódia e base de conhecimento documentadas
5. Atribuição e IPs (para o BO)
Autor: máquina MOM-ESC-TI-01 (domínio MOMEACAI.local, DC MOM-SER-AD-01), conta MOMEACAI\tecnologia, perfil maikon.doglas, MSA suporte@acaiteria.com.br. O log prova máquina+conta+IP; a pessoa física depende dos logs do AnyDesk.
IP do pivô: 181.224.84.98 (MOM) → sessões 476/477/3; usuários provados por hash (1029): "call center 03" e administracao.
Outros RDP externos: 181.224.86.101 (ana.pop/ana.xavier, legítimo), 38.224.188.14 ("Pablo Ed Saude").
Força bruta: 62.164.177.27/.28, 88.214.25.123/.125, 112.170.79.190 (externas); fe80::, 192.168.1.110 (internas).
Ferramentas/artefatos: CrystalDiskInfo (winget 17:48), pendrive bootável de reset, Tenorshare, AnyDesk/TeamViewer, cache de bitmap RDP (EVIDENCIA_RDP_POP-SERVER-02.png).
Contato de extorsão: datastore@cyberfear.com.
Integridade: logs da MOM sem 1102 e com EventRecordID contínuo; hashes SHA-256 em todas as coletas.
6. Estado atual e caminhos
Chave de 48 dígitos (E7D36899): não recuperável de nenhuma fonte física (TRIM destruiu o TXT); sem escrow em AD, conta Microsoft (Flávio) ou nuvem. Pagefile do servidor (9,1 GB) varrido inteiro = negativo.
Senha BitLocker (0x8, F68473D0): gerada no ChatGPT (24 chars, 17:08) — só no servidor da OpenAI (prompt provado no IndexedDB; resposta nunca gravada localmente).
Dados do E:: cifragem "somente espaço usado", ~27 min até o reboot → possível texto claro residual p/ carving (páginas de 8 KB do PostgreSQL, Office/PDF) — avaliar com perito, sobre imagem.
PostgreSQL: indícios fortes de que não foi apagado (subiu normal às 17:21; sem 7031/7034 até o bloqueio) — íntegro, só trancado.
🎯 Google Drive (NOVO): rastro .tmp.driveupload em Documentos Compartilhados no pagefile → possível cópia na nuvem dos documentos trancados. Checar a conta Google de sync da clínica — caminho de recuperação de dados de maior valor, independe da chave.
Backup disponível: sintec1 de 21/08 (defasagem ~40 dias) — caminho operacional imediato; checar também réplica do lado SINTEC.
Caminhos para os dados (ordem de valor): (1) Google Drive da clínica (se a sync existia); (2) restaurar backup 21/08 já; (3) conta ChatGPT com autorização da Wanessa; (4) quebrar a senha 0x8 (hashcat + regras); (5) BO/jurídico — autor identificado, pode compelir a entrega.
Jurídico/LGPD: dados de saúde (art. 11 LGPD) → avaliar ANPD/titulares; crime (Lei 12.737/2012) → BO em delegacia de crimes cibernéticos; perícia licenciada p/ o cache RDP (dispositivo de terceiro).
Pendências técnicas: varredura do pendrive (pen mom); cache RDP Cache0001.bin; Prefetch/SRUM; logs do AnyDesk (resolve "quem"); checar origem dos PDFs BBFMA/AMEX; checar o Google Drive.
Achado lateral de segurança: .pfx do SINTEC com senha no nome do arquivo (D:\Program Files (x86)\SINTEC\certificado\).

Documentos relacionados: inventario_evidencias_EVIDENCIA_POP.md, relatorio_sessao_2026-10-04_consolidado.md, laudo_sessao_20261004_bitlocker_POP-SERVER-02.md, relatorio_sessao_edb_recuperacao_dados.md, laudos (Claude Docs) do incidente BitLocker e da MOM-ESC-TI-01, linha_do_tempo_ataque_POP-SERVER-02.md, analise_cache_rdp_MOM-ESC-TI-01.md, plano_recuperacao_chave_bitlocker_E7D36899.md, plano_investigacao_agressiva.md, aquisicao_MOM-ESC-TI-01.md.
