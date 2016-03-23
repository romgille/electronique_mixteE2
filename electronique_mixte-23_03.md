---
  title: Chapitre 1. Structure d'une chaîne d'acquisition de données, Shannon et quantification
  author: Romain Gille
  date: 23/03/2016
  geometry: margin=1in
...

On avait obtenu

$$V^{*}(f) = \dfrac{1}{T_e} \sum\limits_h V(f - k f_e)$$
$$V^{*}_B(f) = V^{*}(f) . TF(\text{Rect}_{T_e})$$
$$TF(\text{Rect}_{T_e}) = \int \text{Rect}_{T_e} e^{-j2\pi ft} dt$$
$$TF(\text{Rect}_{T_e}) = - \dfrac{1}{j2\pi f} {[e^{-j2\pi ft}]}_0^{T_e}$$
$$= - \dfrac{1}{j2\pi f} (e^{-j2\pi f T_e} - 1)$$
$$= - \dfrac{1}{j2\pi f} e^{- j\pi f T_e} (e^{-j \pi f T_e} - e^{j \pi f T_e})$$
$$= T_e e^{- j\pi f T_e} . \dfrac{sin(\pi f T_e)}{\pi f T_e}$$
$$= T_e e^{-j \pi f T_e} . \text{ sinc}(\pi f T_e)$$

$$V^{*}_B(f) = \dfrac{1}{T_e} \sum\limits_k V(f - k f_e) . T_e e^{-j \pi f T_e}.
  \text{ sinc}(\pi f T_e)$$
$$V^{*}_B(f) = e^{-j \pi f T_e} \text{ sinc}(\pi f T_e)
  \sum\limits_k V(f - k f_e)$$

Le filtre de lissage permet de restituer le signal avant échantillonnage. Ses
caractéristiques (fréquence, ordre, type, ...) doivent être choisis en fonction
des fréquences ($f_e - f_{\text{max}}$) à atténuer. Quand on augmente $f_e$ par
rapport à $f_{\text{max}}$, la variation de fréquence est plus grande et l'ordre
du filtre peut être plus faible et réciproquement.  
Si on augmente $f_e$, les CAN et CNA doivent fonctionner plus rapidement.

Pour simplifier la phase de restitution du signal, dans un lecteur CD il est
souvent utilisé le sur-échantillonnage. Le CD a été échantillonné à
$f_e = 44.1 kHz$ et il est courant de sur-échantillonner d'un facteur 4 soit
$f'_e = 4f_e = 176.4 kHz$.  
Lors de la reproduction du signal, il est maintenu (le signal) pendant une durée
de $\dfrac{T_e}{4}$.

$$V^{*}_B(f) = e^{-j \pi f {T_e \over 4}}
  \text{ sinc}\left(\pi f {T_e \over 4}\right) \sum\limits_k V(f - k 4 f_e)$$


# Calcul du rapport signal à bruit en fonction du nombre de bits

Les convertisseurs CAN et CNA numérisent le signal sur un nombre de bits donné.
Ce nombre doit être choisi en fonction de la précision du signal à restituer.
Ce rapport signal à bruit est :

$$S/B = 10 \log\left(\dfrac{\text{Puissance signal}}{Pbruit}\right)$$

Quand on échantillonne le signal, on introduit un bruit d'échantillonnage.  
Le bruit d'échantillonnage n'a pas d'incidence sur la restitution du signal
lorsqu'il est plus faible que le bruit du signal avant échantillonnage.  
Il revient au même de dire que le rapport signal à bruit engendré par la
numérisation doit être supérieur au rapport signal à bruit du signal à traîter :

$${(S/B)}_{\text{numérisation}} > {(S/B)}_{\text{signal à traîter}}$$

Par exemple :

${(S/B)}_{\text{CD}} > {(S/B)}_{\text{concert}}$

Calculons le rapport signal à bruit engendré par la numérisation :
$$P_{signal} = \dfrac{V^2}{R} = \left(\dfrac{V_{PE}}{2\sqrt{2}}\right)^2
  = \dfrac{V_{PE}^2}{8}$$

Le bruit introduit par la quantification est la différence entre la
caractéristique idéale et la réelle ($\epsilon$)

$$P = \dfrac{1}{T} \int\limits_{0}^T v^2(t) dt$$

$$\epsilon = \dfrac{\dfrac{q}{2}}{\dfrac{T_e}{2}} t = \dfrac{q}{T_e} t$$
$$P = \dfrac{1}{\dfrac{T_e}{2}} \int\limits_{0}^{T_e / 2}
  \left(\dfrac{q}{T_e} t \right)^2 dt$$
$$P = \dfrac{2 q^2}{{T_e}^3} \dfrac{1}{3} \dfrac{{T_e}^3}{8}$$
$$P = \dfrac{q^2}{12}$$
