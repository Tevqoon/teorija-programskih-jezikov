# Prestrezniki učinkov

## Težave z monadami

Monade elegantno opisujejo denotacijsko semantiko učinkov, vendar imajo nekaj pomanjkljivosti:

- Sama monada opisuje le semantiko tipov ter enoto in veriženje, izpusti pa specifične operacije, ki jih potrebujemo za opis učinkov (npr. `get`, `set`, `raise`, `try`, `choose`). Vsako izmed njih moramo podati naknadno.
- Iz istega razloga težko podamo splošno operacijsko semantiko.
- Monade se poleg tega težko kombinirajo: če sta $T_1, T_2 : \mathbf{Set} \to \mathbf{Set}$ monadi, njuna kompozicija $T_1 \circ T_2$ na splošno ni monada. Enoto se sicer da sestaviti:

    $$\eta^{T_1 \circ T_2}_X : X \to T_1(T_2 X), \qquad \eta^{T_1 \circ T_2}_X(x) = \eta^{T_1}_{T_2 X}\bigl(\eta^{T_2}_X(x)\bigr)$$

    Težava nastopi pri veriženju, kar je lažje videti pri ekvivalentni predstavitvi z množenjem $\mu_X : TTX \to TX$: iz $\mu^{T_1}$ in $\mu^{T_2}$ dobimo preslikavo $T_1(T_1(T_2(T_2 X))) \to T_1(T_2 X)$, potrebovali pa bi pa $T_1(T_2(T_1(T_2 X))) \to T_1(T_2 X)$.

Zato se osredotočimo na manjkajoči opis specifičnih operacij. S študijem operacij se ukvarja področje algebrajskih teorij, ki v splošnosti preiskuje strukture, kot so polgrupe, grupe, kolobarji, …

## Signature in algebrajske teorije

**Def.** *Signatura* $\Sigma$ za algebrajsko teorijo je seznam operacij in njihovih členosti (= arity):
$$\Sigma = \mathrm{op}_1 : n_1,\ \dots,\ \mathrm{op}_k : n_k$$

**Primer.** $\Sigma_{\text{grupa}} = \{\underbrace{m : 2}_{\text{množenje}},\ \underbrace{i : 1}_{\text{inverz}},\ \underbrace{e : 0}_{\text{enota}}\}$

Iz spremenljivk in operacij iz $\Sigma$ sestavljamo izraze, npr. $m(i(x), m(m(y, e()), z))$. Formalno jih definiramo induktivno. Za kontekst $\Gamma = x_1, \dots, x_n$ uvedemo sodbe $\Gamma \vdash_\Sigma t$ s pravili:

$$
\infer{
    x \in \Gamma
}{
    \Gamma \vdash_\Sigma x
} \qquad
\infer{
    \mathrm{op} : n \in \Sigma \quad
    (\Gamma \vdash_\Sigma t_i)_{i=1}^n
}{
    \Gamma \vdash_\Sigma \mathrm{op}(t_1, \dots, t_n)
}
$$

**Def.** *Algebrajska teorija* nad signaturo $\Sigma$ je množica enačb med izrazi.

**Primer.** Teorija za grupe:

$$
\begin{align*}
  x, y, z \vdash m(x, m(y, z)) &= m(m(x, y), z) \\
  x \vdash m(x, e()) &= x \\
  x \vdash m(e(), x) &= x \\
  x \vdash m(x, i(x)) &= e() \\
  x \vdash m(i(x), x) &= e()
\end{align*}
$$

**Primer.** Teorija za polmreže, kjer vzamemo signaturo $\Sigma = \{\vee : 2\}$:

$$
\begin{align*}
  x \vee (y \vee z) &= (x \vee y) \vee z \\
  x \vee y &= y \vee x \\
  x \vee x &= x
\end{align*}
$$

**Neprimer.** Obsegi: ker je inverz za množenje definiran samo na neničelnih elementih. Kmalu bomo tudi dokazali, da ne obstaja nobena teorija, ki bi opisovala obsege.

## Interpretacije in modeli

**Def.** *Interpretacija* signature $\Sigma$ je množica $X$ skupaj s funkcijo $\mathrm{op}_X : X^{n} \to X$ za vsak $(\mathrm{op} : n) \in \Sigma$.

Interpretacija nam omogoča interpretirati vsak izraz $x_1, \dots, x_n \vdash t$ s preslikavo $\itp{t} : X^n \to X$, definirano kot:

$$
\begin{align*}
  \itp{x_1, \dots, x_n \vdash x_i}(a_1, \dots, a_n) &= a_i \\
  \itp{\mathrm{op}(t_1, \dots, t_n)} &= \mathrm{op}_X(\itp{t_1}, \dots, \itp{t_n})
\end{align*}
$$

**Def.** *Model* algebrajske teorije $M$ je taka interpretacija njene signature $\Sigma$, ki zadošča vsem enačbam, torej: $\itp{t} = \itp{t'}$ za vsako enačbo $t = t'$ v teoriji.

**Primer.** Vsaka grupa $G$ je model teorije za grupe.

**Trditev.** Za poljubna modela $(X, (\op_X)_{\op \in \Sigma})$ in $(Y, (\op_Y)_{\op \in \Sigma})$ lahko definiramo model na $X \times Y$, pri čemer operacijo $(\op : n) \in \Sigma$ interpretiramo kot:
$$
  \op_{X \times Y}((x_1, y_1), \dots, (x_n, y_n)) = (\op_X(x_1, \dots, x_n),\ \op_Y(y_1, \dots, y_n))
$$

Posledica tega je, da ne obstaja algebrajska teorija za obsege, saj bi za poljubna obsega $X$ in $Y$ dobili obseg $X \times Y$, ki pa ni obseg, saj je $(1, 0) \cdot (0, 1) = (0, 0)$, torej imamo delitelje niča.

**Def.** *Homomorfizem* med modeloma $X$ in $Y$ je taka preslikava $h : X \to Y$, da za vsako operacijo $(\op : n) \in \Sigma$ velja

$$
  h\bigl(\op(a_1, \dots, a_n)) = \op(h(a_1), \dots, h(a_m))
$$

