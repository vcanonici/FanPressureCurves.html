# Simulador Temperatura × Velocidade

Calculadora que permite definir curvas temperatura → PWM para cada grupo de ventoinhas e analisar o impacto no fluxo líquido e na pressão da caixa.

## Entradas

- **Grupos de ventoinhas** (botão *Add*):
  - `name`: identificação do grupo.
  - `role`: `Intake` ou `Exhaust`.
  - `n`: quantidade de ventoinhas idênticas.
  - `Qfree`: fluxo livre por ventoinha (CFM) utilizado na interpolação linear.
- **Curva temperatura → %**: pontos \((T, p)\) adicionados para o grupo selecionado, onde \(p\) é a velocidade em %.
- **Resistência R**, **volume/ dimensões** e **fator de fuga** seguem as mesmas definições da calculadora de fluxo × velocidade.
- **Faixa de temperatura** (`tmin`, `tmax`) e **passos** determinam a discretização da malha de cálculo.

## Modelo matemático

1. O volume da caixa produz uma resistência efetiva \(R_{ef}\) idêntica à calculadora anterior.
2. Cada curva é ordenada por temperatura e interpolada linearmente. Para uma temperatura \(T\), obtém-se a velocidade normalizada \(s(T) = \text{clamp}(p(T)/100)\).
3. O fluxo do grupo é calculado por \(Q_{grupo}(T) = n \cdot Q_{livre} \cdot s(T)\).
4. O fluxo líquido e a pressão seguem \(Q_{net} = Q_{in} - Q_{out}\) e \(P_{box} = R_{ef} \cdot Q_{net}^2 \cdot \text{sign}(Q_{net})\).

## Saídas

- Gráfico com fluxo individual por grupo, pressão positiva/negativa e linha de pressão versus temperatura.
- Exportação CSV contendo temperatura, fluxos individuais, agregados, pressão e metadados (\(R_{ef}\), volume, dimensões e fator de fuga).
- Recursos de salvar/carregar perfis de grupos e sessões completas em JSON.

## Como usar

1. Cadastre os grupos de ventoinhas e selecione um para editar a curva.
2. Insira os pontos de temperatura → velocidade desejados e clique em *Add point*.
3. Ajuste resistência, volume e demais parâmetros globais.
4. Pressione **Plot** para atualizar o gráfico ou **Export CSV** para gerar a planilha.
5. Utilize o separador vertical para redimensionar a área de controles conforme necessário.
