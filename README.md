# BioInova STOP! — versão 4 (final para a oficina)

## O que mudou na v4
- **Desempate por tempo de resposta.** Ordem do ranking: pontos → menor tempo → apelido. O tempo é medido por seção: quantos segundos depois do 1º envio da seção a pessoa terminou (quem não respondeu conta o pior tempo da seção). Só entra quando há empate de pontos; aparece no painel como "desempate 3,2 s". Atenção: depende do sinal do celular, por isso é apenas desempate.
- **Respostas "R" e "C" agora valem** (antes eram recusadas por ter 1 letra).
- **Modelo 100% automático.** O monitor não escolhe nada: o app usa o modelo calibrado (MPNet), troca sozinho para o menor (MiniLM) em aparelho com pouca memória ou se o principal falhar, e cai para análise só por texto se nada carregar. Mostra apenas "preparando (xx%)" e "análise ativa".
- **Limpeza automática de downloads antigos** do modelo (Cache Storage) assim que o modelo em uso carrega. No modo desenvolvedor há o botão "🧹 Limpar downloads antigos".
- **Pré-aquecimento:** os vetores das respostas são calculados durante a rodada, então o resultado sai mais rápido ao encerrar.
- **Laboratório com teste final:** +285 respostas de relevância (19 temas novos, frases, erros de digitação) e +29 perguntas de agrupamento que nunca são usadas para escolher parâmetros; a nota geral agora vem desse teste.

## Como atualizar (quem já tem a v3 rodando)
1. Supabase → SQL Editor → cole `supabase_migracao_v4.sql` → Run. **Não apaga dados.**
2. Troque o `index.html` no GitHub (commit). Aguarde 1 min e recarregue (Ctrl+F5).
Instalação do zero: use `supabase_setup.sql` (apaga tudo) e depois o `index.html`.

## Antes da oficina
- Abra o painel do monitor **uma vez com internet boa** (o modelo baixa só neste aparelho e fica guardado).
- Troque a senha geral `AdminPET-troque` e o convite `PET2026` em `#admin`.
- Faça um ensaio (botão "Simular turma" no modo `#dev`) e depois "Remover simulados".

---

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
Método "contraste": relevância = proximidade ao tema da pergunta menos a maior proximidade a assuntos do cotidiano sem relação. Calibração (54 respostas, 18 fora do assunto): separação 0,97 (tema sozinho 0,86; usar as demais respostas 0,65). Valores medidos na validação do laboratório (MPNet): corte -35%, limiar de agrupamento 55%. O corte extra para ideias únicas foi testado e o treino escolheu 0. O mediador devolve pontos com "Aceitar".

## Pontuação e qualidade das respostas (automático, sem custo de tempo)
- Ruído evidente (teclado batido, "kkkk", sem vogais, "não sei", "nada"…) vale 0 e nunca forma consenso.
- Modelo semântico opcional (transformers.js, carregado **só no navegador do mediador**, ~280 MB na 1ª vez (MPNet; se não carregar, usa o MiniLM de ~120 MB), depois fica em cache): agrupa por significado ("IA" = "inteligência artificial", "telemedicina" = "consulta remota") e zera respostas fora do assunto da pergunta. Se não carregar, o app usa só comparação de texto.
- O painel do monitor mostra só o essencial. Ferramentas de teste (simular turma, sliders, testar modelo, **Calibração**) ficam no bloco "Desenvolvedor", visível ao abrir qualquer painel pelo `#admin` ou após visitar `.../#dev` (desliga com `#dev-off`).
- **Calibração:** no bloco Desenvolvedor, "Carregar exemplos" → "Calcular" roda um conjunto de teste com o modelo real, compara 4 métodos de relevância (tema, colegas, máximo dos dois e **contraste**, o padrão) e sugere o melhor método/corte e o melhor limiar de agrupamento; "Aplicar valores" usa na sessão. Passe-me os números para virarem o padrão de todos.
- **Laboratório (só desenvolvedor):** "Rodar laboratório" usa uma base simulada embutida (239 respostas de relevância, 24 perguntas de agrupamento, separadas em treino e validação), escolhe método, cortes e limiar no treino, mede na validação e compara até 4 modelos (MPNet, MiniLM, DistilUSE; o E5 foi descartado por usar outra escala de similaridade). "Importar correções da sala" traz as correções reais do monitor para as mesmas caixas. "Aplicar valores" usa o melhor; para o modelo vale a próxima abertura do painel.
- Na Auditoria, "Aceitar" devolve os pontos de uma resposta marcada.
- Dica: abra o painel uma vez com internet boa antes da oficina para o modelo ficar em cache.

## Resultado do laboratório (validação, MPNet)
Separação de relevância 0,94 (pega 95% do fora do assunto, zera 7 de 54 respostas boas); agrupamento F1 0,78. MiniLM: 0,89 e 0,67. Nomes de ferramentas (Tableau, Stata, SQL…) e siglas entram por glossário.
