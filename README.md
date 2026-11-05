# FII Planner: simulador de renda com fundos imobiliários (Excel)

Ferramenta em Excel, com cara de aplicativo, que simula investimentos mensais em fundos imobiliários (FIIs). Feita como desafio da DIO.

Arquivo: `FII_Planner_Melissa.xlsx`

> Todos os valores do arquivo (salário, aporte, prazo, taxa e percentuais) são **exemplos**. Nada aqui é recomendação de investimento.

## Perguntas que a ferramenta responde

Todas aparecem na aba **Simulador**.

| Pergunta | Onde aparece |
|---|---|
| Quanto investir por mês? | Seção 2, célula `C13` (`aporte`). A sugestão de 30% do salário está em `C9`. |
| Por quantos anos? | Seção 2, célula `C14` (`anos`) |
| Qual a taxa de rendimento mensal? | Seção 1, célula `C7` (`taxa_mensal`) |
| Quanto de patrimônio vai acumular? | Seção 2, célula `C16` (`patrimonio`) |
| Quanto vai receber de dividendos por mês? | Seção 2, célula `C17` (`dividendos`) |

Além disso:
- **Seção 3:** projeção do patrimônio e dos dividendos em 2, 5, 10, 20 e 30 anos.
- **Seção 4:** divisão do aporte entre seis tipos de FII, conforme o perfil escolhido na lista suspensa (`C10`), com gráfico de pizza.
- **Seção 5:** segunda simulação que usa a sugestão de 30% do salário no lugar do aporte informado e mostra a diferença nos dividendos.

## Como `VF` e `PROCV` entram nos cálculos

**VF** (valor futuro) calcula o patrimônio acumulado:

```
=VF(taxa_mensal; anos*12; -aporte)
```

- `taxa_mensal`: rendimento mensal da carteira.
- `anos*12`: número de meses de aportes.
- `-aporte`: valor investido por mês, negativo porque é dinheiro que sai do bolso.

Os dividendos mensais são `patrimônio × taxa_mensal`. Nos cenários, a mesma fórmula é repetida com o prazo em `$B22`, `$B23` etc., com `$` para a referência não se perder ao arrastar.

**PROCV** traz o percentual de cada tipo de fundo para o perfil escolhido, usando uma **chave composta** (perfil + tipo de fundo):

```
=PROCV(perfil & B30; tabela_perfis; 4; FALSO)
```

Na aba **Apoio**, a coluna A monta a chave (`=B2&C2`, por exemplo `ModeradoLogística`). O `PROCV` procura essa chave e devolve a coluna 4, que é o percentual. O `FALSO` exige correspondência exata.

## Intervalos nomeados

| Nome | Referência | O que guarda |
|---|---|---|
| `salario` | Simulador!C6 | Salário mensal |
| `taxa_mensal` | Simulador!C7 | Rendimento mensal da carteira |
| `pct_sugestao` | Simulador!C8 | % do salário sugerido (30%) |
| `aporte_sugerido` | Simulador!C9 | Salário × 30% |
| `perfil` | Simulador!C10 | Perfil escolhido na lista |
| `aporte` | Simulador!C13 | Aporte mensal informado |
| `anos` | Simulador!C14 | Prazo da simulação |
| `patrimonio` | Simulador!C16 | Patrimônio acumulado |
| `dividendos` | Simulador!C17 | Dividendos mensais |
| `tabela_perfis` | Apoio!A2:D19 | Tabela de chave, perfil, tipo e percentual |
| `lista_perfis` | Apoio!F2:F4 | Lista usada na validação de dados |

## Percentuais de cada perfil

Os percentuais abaixo são **exemplos definidos por mim**, sem fonte externa e sem caráter de recomendação. Cada perfil soma 100%, e a aba Apoio confere isso.

| Tipo de fundo | Conservador | Moderado | Arrojado |
|---|---|---|---|
| Lajes Corporativas | 10% | 15% | 20% |
| Logística | 25% | 25% | 25% |
| Shoppings | 15% | 20% | 25% |
| Papel (CRI) | 40% | 25% | 10% |
| Fundo de Fundos | 10% | 10% | 5% |
| Híbrido / Desenvolvimento | 0% | 5% | 15% |
| **Total** | **100%** | **100%** | **100%** |

A lógica: o perfil conservador concentra mais em fundos de papel, e o arrojado em fundos de tijolo e desenvolvimento.

## O que mudei em relação à ferramenta do Expert

- **Layout próprio:** banner com outro nome e seções numeradas (configurações, simulação, cenários, divisão por perfil, simulação com a sugestão), com legenda de cores (azul sobre amarelo para campos editáveis e preto sobre cinza para calculados).
- **Seis tipos de fundo diferentes:** Lajes Corporativas, Logística, Shoppings, Papel, Fundo de Fundos e Híbrido/Desenvolvimento, em vez de Papel, Tijolo, Híbridos, FOFs, Desenvolvimento e Hotelarias.
- **Percentuais e valores de exemplo próprios** (salário, aporte, prazo, taxa e perfis).
- **Taxa única:** uso apenas `taxa_mensal` tanto para o VF quanto para os dividendos.
- **Outra chave composta:** perfil e tipo de fundo concatenados direto, sem hífen, apontando para o intervalo nomeado `tabela_perfis` em vez de colunas inteiras.
- **Nomes de intervalos diferentes** e lista de perfis também nomeada (`lista_perfis`).
- **Conferência na aba Apoio:** soma de cada perfil, para garantir 100%.
- **Segunda simulação** com a sugestão de 30% do salário (ideia de evolução do desafio).

## Evidências

Prints na pasta `prints/`, com a mesma simulação em dois perfis diferentes (aporte, prazo e taxa iguais), mostrando como a divisão do aporte muda.

## Como usar

1. Abra `FII_Planner_Melissa.xlsx` no Excel.
2. Edite apenas as células azuis sobre amarelo.
3. Escolha o perfil na lista suspensa da célula `C10`.
