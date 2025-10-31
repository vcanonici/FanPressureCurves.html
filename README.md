# Fan Pressure Suite

Conjunto de simuladores para estudar o equilíbrio de fluxo e pressão em gabinetes de computador. O repositório foi reorganizado para facilitar a navegação entre as calculadoras e documentar as funções matemáticas utilizadas em cada uma.

## Estrutura

```
.
├── index.html                          # Página inicial estilizada com os atalhos
├── calculators/
│   ├── dynamic-speed/                  # Simulador de fluxo × velocidade
│   │   └── index.html
│   │   └── README.md
│   └── temperature-response/           # Simulador temperatura × velocidade
│       └── index.html
│       └── README.md
└── LICENSE
```

## Calculadoras

- **Simulador de Fluxo / Pressão (`calculators/dynamic-speed/`)** — Usa uma malha de velocidades normalizadas (0–100 %) para calcular o fluxo individual de cada grupo e a pressão interna resultante utilizando a relação quadrática \(P = R_{ef} \cdot Q_{net}^2 \cdot \text{sign}(Q_{net})\).
- **Simulador Temperatura × Velocidade (`calculators/temperature-response/`)** — Cada grupo possui uma curva temperatura → PWM. Para cada ponto de temperatura a aplicação interpola a velocidade, calcula o fluxo líquido e aplica a mesma equação de pressão.

Detalhes completos de entrada, saída e exportação encontram-se nos READMEs de cada calculadora.

## Licença

Projeto licenciado sob [MIT](LICENSE).
