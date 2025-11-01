# Simulador de Fluxo / Pressão

Calculadora para estimar o equilíbrio de pressão intragabinete enquanto as ventoinhas variam de 0 a 100 % da rotação máxima.

## Entradas

- **Grupos de ventoinhas**: cada grupo representa ventoinhas idênticas em paralelo.
  - `n`: quantidade de ventoinhas no grupo.
  - `Qfree`: fluxo livre por ventoinha (CFM) na velocidade de referência.
  - `SPmax`: pressão estática máxima informativa (não altera o cálculo, usada apenas como contexto).
  - `role`: marca o grupo como intake (`in`) ou exaustão (`out`).
- **Resistência base R**: parâmetro global \(R\) em mmH₂O/CFM². Representa a perda de carga causada por filtros, grelhas e outras obstruções.
- **Dimensões/volume da caixa**: o volume pode ser informado pelas dimensões em centímetros ou diretamente em litros. O simulador converte tudo para metros cúbicos.
- **Fator de fuga**: multiplica a resistência efetiva para simular gabinetes mais “abertos” (valores < 1) ou mais selados (valores > 1).
- **Passos**: número de pontos entre 0 % e 100 % utilizados na malha de cálculo.

## Modelo matemático

1. O volume informado gera um fator de correção:
   \[
   R_{ef} = R \cdot \frac{V_{ref}}{V_{box}} \cdot f_{fuga}
   \]
   onde \(V_{ref} = 0{,}05\,\text{m}^3\) (50 L).
2. Para cada passo de velocidade \(s \in [0,1]\) calcula-se o fluxo total do grupo com relação linear simples:
   \[
   Q_{grupo} = n \cdot Q_{livre} \cdot s
   \]
3. Somatório dos fluxos de entrada e saída produz o fluxo líquido \(Q_{net} = Q_{in} - Q_{out}\).
4. A pressão interna estimada assume relação quadrática:
   \[
   P_{box} = R_{ef} \cdot Q_{net}^2 \cdot \operatorname{sign}(Q_{net})
   \]

## Saídas e exportação

- Gráfico com as curvas de fluxo por grupo, envelopes de pressão positiva/negativa e a linha da pressão líquida.
- Texto auxiliar com o valor de \(R_{ef}\) e do volume utilizado.
- Exportação CSV com as colunas:
  - `speed_pct`, valores de 0–100 %.
  - Uma coluna por grupo com o fluxo resultante (CFM).
  - `Q_in`, `Q_out`, `Q_net`, `P_box_mmH2O`, `R_eff_mmH2O_per_CFM2`, `box_volume_L`, `base_R`, `leak_factor`.

## Como usar

1. Ajuste o volume informando dimensões ou litros e observe a atualização da resistência efetiva.
2. Cadastre os grupos de ventoinhas com os parâmetros desejados ou carregue os padrões.
3. Clique em **Plot** para gerar o gráfico ou em **Exportar CSV** para baixar os dados.
4. Utilize o separador central para redimensionar a largura da coluna de controles.
