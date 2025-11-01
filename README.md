# Calculadoras de Pressão Intragabinete

Conjunto de duas ferramentas HTML independentes para estudar o equilíbrio de pressão em gabinetes de computador. As páginas podem
ser abertas diretamente no navegador sem dependências extras.

## Estrutura de arquivos

```
LICENSE
index.html                        # Landing page com atalhos
calculadora_fluxo_pressao.html     # Simulador fluxo × velocidade
calculadora_temperatura_velocidade.html  # Simulador temperatura × PWM
```

## Simulador de Fluxo / Pressão (`calculadora_fluxo_pressao.html`)

Calcula o fluxo líquido e a pressão interna à medida que os grupos de ventoinhas variam de 0 a 100 % da velocidade.

### Entradas principais
- **Grupos de ventoinhas**: quantidade (`n`), fluxo livre por ventoinha (`Qfree`, em CFM), pressão estática de referência (`SPmax`) e o papel (`Intake` ou `Exhaust`).
- **Resistência base R**: parâmetro global em mmH₂O/CFM² que modela filtros, grelhas e dutos.
- **Dimensões/volume da caixa**: informe largura, altura e profundidade (cm) ou diretamente o volume (litros). Tudo é convertido para metros cúbicos.
- **Fator de fuga**: ajusta o quanto o gabinete é selado (valores > 1) ou vazado (valores < 1).
- **Passos de velocidade**: quantidade de pontos usados para construir a malha de cálculo entre 0 % e 100 %.

### Modelo matemático
1. Calcula-se a resistência efetiva com base no volume informado:
   \[
   R_{ef} = R \cdot \frac{V_{ref}}{V_{box}} \cdot f_{fuga}, \quad V_{ref} = 0{,}05\,\text{m}^3
   \]
2. Para cada passo de velocidade \(s \in [0, 1]\) o fluxo do grupo é dado por
   \[
   Q_{grupo} = n \cdot Q_{livre} \cdot s
   \]
3. O fluxo líquido é \(Q_{net} = Q_{in} - Q_{out}\) e a pressão estimada segue a relação quadrática
   \[
   P_{box} = R_{ef} \cdot Q_{net}^2 \cdot \operatorname{sign}(Q_{net}).
   \]

### Saídas
- Gráfico com o fluxo individual por grupo, envelopes de pressão positiva/negativa e linha do fluxo líquido.
- Indicador do volume final e da resistência efetiva calculada.
- Exportação CSV contendo `speed_pct`, uma coluna por grupo, agregados (`Q_in`, `Q_out`, `Q_net`) e os parâmetros globais utilizados.

## Simulador Temperatura × Velocidade (`calculadora_temperatura_velocidade.html`)

Permite definir curvas de temperatura para PWM/percentual de velocidade e avalia o impacto na pressão.

### Entradas principais
- **Grupos de ventoinhas**: mesmo conjunto de parâmetros do simulador anterior.
- **Curva temperatura → velocidade**: pontos \((T, p)\) em °C e %, ordenados automaticamente e interpolados linearmente.
- **Resistência, volume e fator de fuga**: compartilham o mesmo modelo do simulador de fluxo × velocidade.
- **Faixa de temperatura**: limites mínimo e máximo (°C) e número de passos para a discretização da malha.
- **Perfis salvos**: suporte para exportar/importar JSON com grupos e sessões completas.

### Modelo matemático
1. Calcula-se \(R_{ef}\) conforme a expressão anterior.
2. Cada curva gera uma função velocidade \(s(T)\) normalizada a partir da interpolação linear dos pontos.
3. O fluxo por grupo é \(Q_{grupo}(T) = n \cdot Q_{livre} \cdot s(T)\).
4. Fluxo líquido e pressão seguem \(Q_{net} = Q_{in} - Q_{out}\) e \(P_{box} = R_{ef} \cdot Q_{net}^2 \cdot \text{sign}(Q_{net})\).

### Saídas
- Gráfico com fluxo individual, pressão positiva/negativa e linha de pressão versus temperatura.
- Exportação CSV com temperatura, fluxos, agregados, pressão e metadados da sessão.
- Controles de salvar/carregar curvas e sessões para facilitar comparações.

## Uso rápido
1. Abra `index.html` para navegar entre as calculadoras.
2. Para cada simulador, ajuste a geometria/volume da caixa e cadastre os grupos de ventoinhas.
3. Gere o gráfico com **Plot** e exporte os dados conforme necessário.

## Licença

Distribuído sob a licença [MIT](LICENSE).
