# BioInova STOP! — métricas de execução (v4)

## Desempenho (medido em teste automatizado, banco PostgreSQL real; rede local, então a latência de internet não está incluída)
| Medida | Resultado |
|---|---|
| 100 participantes entrando ao mesmo tempo | 2,7 s no total, 0 falhas |
| 100 participantes enviando 5 respostas cada (500 respostas) ao mesmo tempo | 3,2 s no total, 0 falhas |
| Processar 500 respostas (agrupar, pontuar, filtrar ruído; modo só texto) | ~30 ms |
| Calcular o ranking | < 10 ms |
| Fluxo completo (cadastro, roteiro, largada de 5 s, envio, encerramento, pontuação, ranking, senha errada) | 15 de 15 verificações ok |
| Desempate por tempo: painel do monitor × resultado no celular | mesma ordem nos dois |

## Qualidade da análise inteligente (medida no laboratório com o modelo real, no seu navegador)
| Medida | MPNet (padrão) | MiniLM (reserva) | Só texto |
|---|---|---|---|
| Separação de respostas fora do assunto (validação, 1,00 = perfeita) | 0,94 | 0,89 | — |
| Fora do assunto detectado | 95% | 95% | — |
| Respostas boas zeradas por engano | 7 de 54 (13%) | 10 de 54 (19%) | — |
| Agrupamento por significado (F1, validação) | 0,78 | 0,67 | 0,26 |

Ressalvas honestas: esses números vêm de uma base simulada; a nova base de teste final (dados nunca usados para ajustar nada) está embutida no laboratório e ainda precisa ser rodada no seu navegador (modo `#dev` → Rodar laboratório) para gerar a nota final. Dados reais da oficina de hoje são a validação definitiva.

## Segurança
Tabelas fechadas ao público; todo acesso passa por funções do servidor; senhas guardadas com hash; tempo e largada controlados pelo servidor; chave pública (anon) não dá acesso a nada além das funções.

## Custo
R$ 0: GitHub Pages + Supabase gratuito; o modelo roda só no navegador do monitor e nenhum participante baixa nada.
