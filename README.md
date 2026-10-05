# Fon-GPT

Un mini assistant vocal qui comprend et répond en fongbe. Tu poses une question en fongbe
(écrite ou parlée), il te répond en fongbe, à l'écrit et à l'oral.

Projet de démonstration, réalisé avec des modèles ouverts sur Google Colab (GPU gratuit).

## Démo

[Lien vers la vidéo de démonstration]

## Comment ça marche

1. L'écoute transforme la voix fon en texte fon.
2. La traduction passe le texte du fongbe au français.
3. Un LLM répond en français.
4. La traduction repasse la réponse en fongbe.
5. Un modèle de voix lit la réponse.

On passe par le français parce que les LLM ouverts comprennent mal le fongbe.

Deux façons de poser une question : écrire en fongbe ou parler dans le micro.

## Lancer le projet

1. Ouvre le carnet dans Colab : [![Ouvrir dans Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/rudigerroger/fon-gpt/blob/main/Fon_GPT.ipynb)
2. Menu Exécution > Modifier le type d'exécution : choisis le GPU T4.
3. Crée une clé gratuite sur console.groq.com/keys, puis ajoute-la dans les Secrets de
   Colab (icône clé à gauche) sous le nom `GROQ_API_KEY`, avec l'accès au notebook activé.
4. Exécution > Tout exécuter.
5. Ouvre le lien `gradio.live` affiché à la fin, dans un nouvel onglet.

Colab gratuit efface tout après une période d'inactivité : il faut alors tout relancer.

## Modèles utilisés

 ### Étape  ->  Modèle utilisé              ->  Auteur  ->  Licence 

 Écoute     ->  omniASR_CTC_300M            ->  Meta AI ->  Apache-2.0
 
 Traduction ->  nllb-200-distilled-1.3B     ->  Meta AI ->  CC-BY-NC-4.0 
 
 Cerveau    ->  gpt-oss-20b, via l'API Groq ->  OpenAI  ->  Apache-2.0 
 
 Voix       ->  mms-tts-fon                 ->  Meta AI ->  CC-BY-NC-4.0 

## Choisir la taille des modèles

L'écoute (Omnilingual ASR) existe en plusieurs tailles. Plus un modèle est gros, plus il est
précis en général, mais plus il prend de place en mémoire. D'après la documentation on a entre autres :

omniASR_CTC_300M, omniASR_CTC_1B, omniASR_CTC_3B, omniASR_CTC_7B

Le GPU gratuit de Colab (T4) offre environ 15 Go de mémoire, partagée avec la traduction et la
voix. Les petits modèles passent sans problème, les plus gros risquent de saturer la mémoire.
Ce carnet utilise `omniASR_CTC_300M` ; pour en changer, modifie la ligne `MODELE_ECOUTE` dans
la cellule des réglages.

La traduction suit le même principe : `nllb-200-3.3B` est plus gros que le `1.3B` utilisé ici,
et plus lourd à charger sur Colab gratuit.

## Le LLM et la clé API

Le carnet envoie les questions à un modèle hébergé par Groq. Il faut donc une clé gratuite,
à ajouter dans les Secrets de Colab.

Il est possible de faire tourner un LLM directement dans Colab, sans clé, mais il occupe
plusieurs Go de mémoire GPU en plus des autres modèles, ce qui dépasse souvent la capacité
du GPU gratuit.

## Limites

- **La traduction du fongbe vers le français est faible.** C'est le principal défaut du
  projet. Un mot mal compris peut changer le sens de la question.
- **La traduction peut dériver** : le texte obtenu est fluide mais ne dit pas toujours la
  même chose que l'original. Les nombres sont mal traduits, donc le LLM est réglé pour les
  écrire en toutes lettres.
- **Les erreurs s'additionnent** : l'écoute, deux traductions et le LLM peuvent chacun se tromper.
- **La qualité de l'enregistrement compte** : un micro d'ordinateur ou une pièce bruyante
  dégrade l'écoute.
- **La voix est unique et robotique**, et le rythme du fongbe n'est pas toujours bien suivi.
- **Le LLM n'a pas accès à internet** : ses réponses peuvent être fausses ou dépassées.
  Il est réglé pour refuser les conseils de santé.
- **Les questions sont envoyées à Groq** pour obtenir la réponse.
- L'audio est limité à 30 secondes.

## Pistes d'amélioration

- Réentraîner la traduction sur des paires fongbe-français.
- Collecter des enregistrements fongbe avec leur texte, pour améliorer l'écoute et la voix.
- Tester les modèles plus grands cités plus haut, sur une machine plus puissante.

## Crédits et licences

Omnilingual ASR, NLLB et MMS-TTS sont des travaux de Meta AI. gpt-oss est publié par OpenAI
et hébergé par Groq. La voix (MMS-TTS) et la traduction (NLLB) sont sous licence
non commerciale : ce projet est une démonstration gratuite. Vérifie les licences sur les pages officielles des modèles avant de
réutiliser le projet.

Auteur : Roger NOUHOEFLIN 
