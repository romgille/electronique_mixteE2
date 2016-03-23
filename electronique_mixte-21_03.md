---
  title: Chapitre 1. Structure d'une chaîne d'acquisition de données, Shannon et quantification
  author: Romain Gille
  date: 21/03/2016
  geometry: margin=1in
...

\newpage

# Structure d'une chaîne d'acquisition et restitution

Nous allons étudier comment placer les différents blocs électroniques pour faire
l'acquisition du signal et sa restitution.

## Étapes d'acquisition

$$\text{Capteur} \rightarrow \text{Étage de conditionnement} \rightarrow
\text{Filtre anti repliement (Anti aliasing filter)} $$
$$\rightarrow \text{Echantillonneur bloqueur (Sample and Hold)} \rightarrow
\text{CAN (ADC)}$$

L'**étage de conditionnement** adapte le signal de sortie du capteur pour
délivrer une tension en sortie.

Le **filtre anti repliement** est un passe-bas et il a pour rôle de réduire les
fréquences au dessus d'une fréquence maximale. On pourra ainsi échantilloner le
signal et respecter le théorème de Shannon.

L'**échantillonneur bloqueur** prend des échantillons tous les
$\dfrac{1}{f_e}=T_e$ et maintien la tension constante entre deux
échantillonnages successifs. La tension est maintenue constante pour que la
conversion par le CAN (convertisseur analogique numérique) soit précise.

Le **CAN** délivre un mot numérique sur $n$ bits après un temps de conversion.  
Le *quantum* est le plus petit écart entre deux valeurs pouvant être convertie.
En dessous de cette valeur, le CAN ne fait pas la différence entre deux valeurs,
elle ne seront pas différenciés (graphe en escalier).

## Étapes de la chaîne de restitution

$$\text{CNA} \rightarrow \text{Filtre de lissage}$$

Ce **convertisseur numérique analogique** délivre un signal analogique qui est
cadencé par une horloge.

Le **filtre de lissage** est un filtre passe-bas. Il "lisse" le signal et il
réduit les effets de l'échantillonnage.

\newpage

# Calcul du spectre du signal échantillonné

## Rappels mathématiques

### Transformée de Fourrier

$$\text{spectre} \rightarrow V(f) = \int v(t) e^{-j 2 \pi f t} dt$$
$$\rightarrow v(t) = \int V(f) e^{j 2 \pi f t} df$$

### Convolution

Tous les systèmes physiques, électroniques, ... peuvent être décrit par une
convolution.

$$e(t) \rightarrow \fbox{h(t)} \rightarrow s(t) = e(t) * h(t)$$

$$TF[h(t)] = H(f) \text{ : réponse en fréquence}$$
$$TF[s(t)] = S(f) = TF[e(t) * h(t)] = E(f) . H(f)$$

$$f(t) * g(t) = \int f(\tau) . g(t - \tau)d\tau$$

### Peigne de Dirac

$\text{peigne de dirac} = \sum \delta (t - k T_e)$

## Calcul du spectre du signal échantillonné

Pour traiter par un système numérique (ordinateur, DSP, ...), on échantillonne
le signal analogique. On remplace l'infinité de valeurs en un nombre fini sans
entrainer de déformations sur le signal.

$$v^* (t) = v(t) . \sum\limits_n \delta(t - n T_e)$$
$$V^* (f) = V(f) * \dfrac{1}{T_e} \sum\limits_n \delta(f - n f_e)$$

Quand on échantillonne, on périodise le spectre.  
Les fréquences de $V^* (f)$ qui sont au dessus de $f_{\text{max}}$ sont
engendrés par l'échantillonnage. Elles devront être supprimées à la fin du
traitement pour ne pas déformer le signal. Le rôle du filtre de lissage est de
supprimer ces fréquences.  
Pour pouvoir restituer le signal de départ, on doit respecter le théorème de
Shannon avec $f_e >>  2 f_{\text{max}}$.  
Si $f_e < 2 f_{\text{max}}$ il y a recouvrement.

Il existe un compromis pour le choix de $f_e$. Si $f_e$ est très supérieur à
$f_{\text{max}} (Ex: $f_e = 10 f_{\text{max}}$), les CAN et CNA devront
convertir très rapidement la tension. Le coût des CAN et CNA est plus important.
Par contre, l'ordre du filtre de lissage sera plus faible.  
Quand on réduit $f_e$ (Ex : $f_e = 3 f_{\text{max}}$), le coût des CAN et CNA
est réduit mais le filtre de lissage est plus complexe (ordre du filtre plus
élevé).

\newpage

# Calcul du spectre du signal échantillonné et bloqué

Avant le CAN, le signal est maintenu constant donc bloqué.  
$v^{*}_{B}(t)$ : signal échantillonné bloqué.
$v^{*}_{B}(t) = v^*(t) * \text{Rect}_{T_e}$
