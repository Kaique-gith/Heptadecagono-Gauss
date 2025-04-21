import streamlit as st import matplotlib.pyplot as plt import numpy as np import time

st.set_page_config(page_title="Heptadecágono com Régua e Compasso", layout="wide") st.title("Construção do Heptadecágono Regular (17 lados)")

Abas teóricas e interativas

tabs = st.tabs(["Visualização", "História e Teoria", "Construção Geométrica"])

with tabs[0]: st.markdown(""" ### Visualização Interativa Escolha o ponto inicial para desenhar o heptadecágono. O ponto será destacado em vermelho. Você também pode animar a construção. """)

n = 17
start_index = st.slider("Escolha o ponto inicial (1 a 17):", 1, n, 1)

if "step" not in st.session_state:
    st.session_state.step = 0

col1, col2 = st.columns([1,1])
with col1:
    if st.button("Próximo vértice"):
        st.session_state.step = (st.session_state.step + 1) % (n + 1)

with col2:
    if st.button("Reiniciar animação"):
        st.session_state.step = 0

theta = np.linspace(0, 2 * np.pi, n + 1)
x = np.cos(theta)
y = np.sin(theta)

fig, ax = plt.subplots(figsize=(6,6))
circle = plt.Circle((0, 0), 1, fill=False, color='gray', linestyle='--')
ax.add_artist(circle)

for i in range(st.session_state.step):
    a = (start_index - 1 + i) % n
    b = (start_index - 1 + i + 1) % n
    ax.plot([x[a], x[b]], [y[a], y[b]], color='blue')

for i in range(n):
    ax.plot(x[i], y[i], 'ko')
    ax.text(x[i]*1.08, y[i]*1.08, str(i+1), fontsize=10, ha='center')

ax.plot(x[start_index - 1], y[start_index - 1], 'ro', markersize=10, label='Ponto inicial')
ax.legend(loc='upper right')
ax.set_xlim(-1.2, 1.2)
ax.set_ylim(-1.2, 1.2)
ax.set_aspect('equal')
ax.axis('off')

st.pyplot(fig)

with tabs[1]: st.markdown(""" ### História e Contribuição de Gauss Em 1796, com apenas 19 anos, Carl Friedrich Gauss provou que é possível construir um polígono regular de 17 lados com régua e compasso.

Ele mostrou que a raiz complexa \zeta = e^{2\pi i/17} é construtível, o que implica que \cos\left(\frac{2\pi}{17}\right) pode ser obtido por radicais quadráticos.

Isso marcou o renascimento da geometria clássica, sendo a primeira nova construção em mais de dois mil anos desde os gregos.

A fórmula:

\cos\left(\frac{2\pi}{17}\right) = \frac{1}{16} \left( -1 + \sqrt{17} + \sqrt{2(17 - \sqrt{17})} + 2\sqrt{17 + 3\sqrt{17} - \sqrt{2(17 - \sqrt{17})}} \right)

é construtível usando apenas régua e compasso, pois depende apenas de raízes quadradas.
""")

with tabs[2]: st.markdown(""" ### Construção com Régua e Compasso (Resumo)

A construção completa é composta por:

1. Desenhar a circunferência com centro O
2. Marcar o ponto A na circunferência
3. Traçar o diâmetro AB
4. Dividir o raio em 17 partes iguais
5. Obter segmentos construídos com raízes quadradas a partir de proporções derivadas
6. Marcar os 17 vértices consecutivamente com o compasso a partir do ponto inicial
7. Ligar os vértices com segmentos retos

A construção completa pode ser feita com base em expressões trigonométricas e radicais que envolvem \sqrt{17}, e por isso é válida com instrumentos clássicos.
""")

