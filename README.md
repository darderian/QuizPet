# BioInova STOP! — v3 (várias salas, um monitor por sala)

Arquivos: `index.html` (o app) + `supabase_setup.sql` (banco). Publicação: GitHub Pages
(https://darderian.github.io/QuizPet/).

## Instalar / atualizar
1. Supabase → SQL Editor → cole todo o `supabase_setup.sql` → Run. **Atenção:** apaga os dados da versão anterior.
2. Substitua o `index.html` no repositório (commit na branch main).
3. **Logo em seguida** abra `.../#admin` (senha inicial `AdminPET-troque`) e troque a **senha geral** e o **código do convite** (inicial `PET2026`).

## Papéis
- **Admin geral (você):** `.../#admin` — copia o link de cadastro, lista monitores/salas, abre o painel de qualquer sala, redefine senha de monitor, apaga monitor+sala, troca convite e senha geral.
- **Monitor:** recebe o link `.../#cadastro=CODIGO` (não público), escolhe apelido, título da sala e senha. Cada monitor tem **uma** sala (= uma dinâmica, com suas seções). Depois entra por `.../#mediador` com apelido + senha. Sua senha mestra também abre qualquer sala.
- **Participante:** abre o link do app, busca a sala pelo **título ou nome do monitor**, ou escaneia o QR do telão (`#sala=CÓDIGO`). Apelidos só precisam ser únicos dentro da sala.

## Rodada
O monitor aperta Iniciar → os celulares mostram "Prepare-se… 5" (tela carrega antes), depois o cronômetro corre. STOP encurta o prazo (padrão 20 s). Os pontos só são calculados com o painel do monitor aberto.
Botões **Ensaio** (bots) e **Zerar** afetam só a sala do monitor.

## Pontuação e qualidade das respostas (automático, sem custo de tempo)
- Ruído evidente (teclado batido, "kkkk", sem vogais, "não sei", "nada"…) vale 0 e nunca forma consenso.
- Modelo semântico opcional (transformers.js, carregado **só no navegador do mediador**, ~100 MB na 1ª vez, depois fica em cache): agrupa por significado ("IA" = "inteligência artificial", "telemedicina" = "consulta remota") e zera respostas fora do assunto da pergunta. Se não carregar, o app usa só comparação de texto.
- Painel do mediador: *Sensibilidade do agrupamento* (padrão 62%) e *Rigor do filtro de assunto* (padrão 15%; 0 desliga). Botão "Testar modelo" mostra a similaridade de pares de exemplo, para calibrar. Na Auditoria, "Aceitar" devolve os pontos de uma resposta marcada.
- Dica: abra o painel uma vez com internet boa antes da oficina para o modelo ficar em cache.
