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

## Filtro de assunto (calibrado com o modelo real)
Método "contraste": relevância = proximidade ao tema da pergunta menos a maior proximidade a assuntos do cotidiano sem relação. Calibração (54 respostas, 18 fora do assunto): separação 0,97 (tema sozinho 0,86; usar as demais respostas 0,65). Padrão: corte -45%; **ideias únicas (10 pts) exigem corte mais alto (-35%)**, consenso (5 pts) usa o corte normal. O mediador devolve pontos com "Aceitar".

## Pontuação e qualidade das respostas (automático, sem custo de tempo)
- Ruído evidente (teclado batido, "kkkk", sem vogais, "não sei", "nada"…) vale 0 e nunca forma consenso.
- Modelo semântico opcional (transformers.js, carregado **só no navegador do mediador**, ~100 MB na 1ª vez, depois fica em cache): agrupa por significado ("IA" = "inteligência artificial", "telemedicina" = "consulta remota") e zera respostas fora do assunto da pergunta. Se não carregar, o app usa só comparação de texto.
- O painel do monitor mostra só o essencial. Ferramentas de teste (simular turma, sliders, testar modelo, **Calibração**) ficam no bloco "Desenvolvedor", visível ao abrir qualquer painel pelo `#admin` ou após visitar `.../#dev` (desliga com `#dev-off`).
- **Calibração:** no bloco Desenvolvedor, "Carregar exemplos" → "Calcular" roda um conjunto de teste com o modelo real, compara 4 métodos de relevância (tema, colegas, máximo dos dois e **contraste**, o padrão) e sugere o melhor método/corte e o melhor limiar de agrupamento; "Aplicar valores" usa na sessão. Passe-me os números para virarem o padrão de todos.
- **Laboratório (só desenvolvedor):** "Rodar laboratório" usa uma base simulada embutida (239 respostas de relevância, 24 perguntas de agrupamento, separadas em treino e validação), escolhe método, cortes e limiar no treino, mede na validação e compara até 4 modelos (MiniLM, MPNet, E5-small, DistilUSE). "Importar correções da sala" traz as correções reais do monitor para as mesmas caixas. "Aplicar valores" usa o melhor; para o modelo vale a próxima abertura do painel.
- Na Auditoria, "Aceitar" devolve os pontos de uma resposta marcada.
- Dica: abra o painel uma vez com internet boa antes da oficina para o modelo ficar em cache.
