# BioInova STOP! — PET Biomedicina

Dinâmica de inteligência coletiva (estilo "Stop" + ranking Kahoot) para oficinas de inovação.
Site de 1 arquivo (`index.html`) + banco Supabase (`supabase_setup.sql`).

## Instalação (10 min)
1. No Supabase: **SQL Editor → New query**, cole todo o `supabase_setup.sql` e rode (**apaga dados antigos**; pode rodar de novo sem problema).
2. Em `index.html`, confira `CFG.URL` e `CFG.KEY` (a chave **anon** pode ficar pública; a proteção está no banco, que bloqueia as tabelas e só aceita as funções `bio_*`).
3. Troque o `index.html` do repositório (`darderian/QuizPet`) por este. O site fica em https://darderian.github.io/QuizPet/ e o painel do mediador em https://darderian.github.io/QuizPet/#mediador.
4. Entre no painel do mediador com a senha `MonitorPET`. A senha fica guardada no banco (hash). Como ela aparece neste arquivo SQL público, troque por outra (🔑 em "Mais opções") se o repositório for divulgado.
5. **Ensaio:** com o painel aberto, use "🧪 Simular turma" (12 alunos de teste); abra uma rodada e clique de novo para eles responderem. Depois "Remover simulados" ou "Zerar tudo".

## Como conduzir (≈ 16 min dos 25 min)
| Tempo | Etapa |
|---|---|
| 0–3 | Projete o painel do mediador (QR Code); todos entram com apelido |
| 3–10 | Seção 1: rodada (3:30) + STOP (quem completar tudo primeiro encurta o tempo para 20 s) + debate |
| 10–14 | Seção 2: rodada (2:30) + debate |
| 14–23 | Resultados (nuvem azul=consenso 5 pts, laranja=ideia única 10 pts), ranking, pódio |
| 23–25 | Baixar CSV auditável e encerrar |

A barra "plano de tempo" do painel avisa se o roteiro passa do total da oficina (padrão 25 min, editável em "Mais opções").

## Várias seções / temas
**Editar roteiro**: cada seção tem tema central, perguntas, tempo, debate e STOP. Há modelos (rápido, completo, vazio) e dá para importar texto. O mediador abre as seções na ordem que quiser; os pontos somam.

## Palavras ou frases
Os campos aceitam palavra ou frase curta (até 120 caracteres). O agrupamento compara palavras-chave e radicais, então "Automatizar laudos" e "Automação de laudos" se juntam, mas ideias com redação muito diferente podem ficar separadas: ajuste a sensibilidade ou una à mão na Auditoria.

## Auditoria
Aba **Auditoria**: unir, separar ou anular respostas. A aba **Síntese** e o CSV refletem os ajustes.

## Segurança
Tabelas fechadas (RLS + sem permissão para `anon`); tudo passa por funções `SECURITY DEFINER`. Respostas dos outros ficam ocultas durante a rodada. Textos digitados são sempre exibidos como texto (sem XSS). Não há dependências externas (funciona offline após carregar; QR gerado no próprio arquivo).
