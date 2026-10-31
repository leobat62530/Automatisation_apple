# WeWard : valider ses pas automatiquement (iPhone)

Synthèse d'une recherche vérifiée (31/10/2026). Objectif : valider chaque soir
ses **vrais** pas, sans fausser quoi que ce soit.

## Pourquoi l'automatisation Raccourcis échoue

WeWard propose bien une action Raccourcis « Valider mes pas ». Mais quand
l'iPhone est verrouillé, iOS chiffre la base Santé et aucune app ne peut lire les
pas (`HKError.errorDatabaseInaccessible`, documentation Apple). L'automatisation de
22h30 se lance probablement, puis échoue sans message.

Conséquence : **toute solution, même très technique, doit trouver l'iPhone
déverrouillé au moment de la validation**. Un serveur, un VPS ou un Mac dans le
cloud ne peut pas déverrouiller ton iPhone.

> « Accès limité » dans les autorisations Santé n'est pas en cause. C'est le
> nouveau choix d'iOS 27 entre « 30 derniers jours » et « tout l'historique ».
> Les pas du jour sont inclus dans les deux cas.

## Étape 0 : test gratuit avant d'acheter quoi que ce soit

1. Règle l'automatisation pour qu'elle se lance dans 5 minutes.
2. Garde l'iPhone **déverrouillé, écran allumé** jusqu'à ce qu'elle se lance.
3. Vérifie dans WeWard si les pas ont été validés.

- **Validés** : la cause est confirmée, passe à la piste n°1.
- **Pas validés** : le problème vient de l'action WeWard elle-même, et aucun
  matériel ne réglera ça.

## Pistes étudiées

| Piste | Fonctionne ? | Coût | Commentaire |
|---|---|---|---|
| **1. ESP32 en clavier Bluetooth** | Probablement (à tester) | ~10 € | Meilleure option, ni Mac ni PC allumé |
| 2. PC/Raspberry Pi + WebDriverAgent | Seulement si l'iPhone reste allumé | 0–130 € | iOS coupe l'outil ~15 min après l'extinction de l'écran |
| 3. Robot à stylet (servos + ESP32) | Oui | 40–150 € | Gros bricolage |
| Jailbreak | Non | — | Rien pour iPhone récent sous iOS 26.1+ / 27 |
| Recopie de l'iPhone | Non | — | Demande un Mac et n'existe pas dans l'UE |
| Mac loué dans le cloud | Non | ~150 €/mois | L'iPhone n'est pas dans le datacenter |
| Émulateur Android / API WeWard | Exclu | — | Revient à envoyer des pas inventés (fraude) |

### Piste 1 : ESP32 en clavier Bluetooth

Une puce ESP32 (~5–10 €), branchée sur un chargeur USB, se fait passer pour un
clavier Bluetooth appairé à l'iPhone. Elle lit l'heure par Wi-Fi.

1. **22h29** : elle appuie sur une touche pour réveiller l'écran, puis tape le
   code (**une seule tentative**).
2. **22h30** : ton automatisation Raccourcis existante se lance sur un iPhone
   déverrouillé, et la base Santé est lisible.
3. Elle envoie quelques touches inoffensives pour garder l'écran allumé pendant
   la validation.

À tester en premier, car rien ne le garantit sur un iPhone à Face ID sous iOS 27 :

- la saisie du code au clavier Bluetooth. Ça marche sur iPad ; sur iPhone, on n'a
  que des témoignages ;
- une notification sur l'écran verrouillé peut empêcher le pavé de code
  d'apparaître. Mets un mode Concentration actif à 22h29 pour les masquer ;
- règle le clavier matériel de l'iPhone en disposition US (QWERTY), comme
  l'exige la bibliothèque HijelHID_BLEKeyboard.

Risques :

- **le code de l'iPhone est stocké dans la puce** : quiconque la récupère peut
  déverrouiller le téléphone. Active le chiffrement de la mémoire flash et un
  appairage protégé par code ;
- si le code envoyé est faux, iOS impose des délais croissants. Désactive
  « Effacer les données » pendant les tests ;
- un redémarrage nocturne (mise à jour iOS, batterie vide) bloque tout jusqu'au
  prochain déverrouillage manuel.

## Pubs et pop-ups : ne pas les automatiser

- D'après la FAQ WeWard, une pub vidéo peut se lancer quand tu convertis tes pas,
  et c'est voulu.
- D'après la politique de confidentialité, l'algorithme anti-triche, lancé à
  chaque virement ou carte cadeau, cherche les **« visionnages incohérents de
  publicités »** et suspend les comptes concernés.
- Selon la FAQ, la validation manuelle sert à « éviter la triche automatisée ».

Fermer les pubs avec un robot, c'est s'exposer à une suspension et à la perte des
Wards au moment de les encaisser. Il vaut mieux se limiter à l'action officielle
« Valider mes pas ».

## La solution officielle

Dans l'app WeWard, active le **rappel de validation quotidien** à l'heure de ton
choix (22h30). Une notification, Face ID, un appui : ça prend 5 secondes.

## Sources principales

- Apple : [errorDatabaseInaccessible](https://developer.apple.com/documentation/healthkit/hkerror/code/errordatabaseinaccessible),
  [Protecting user privacy](https://developer.apple.com/documentation/healthkit/protecting-user-privacy),
  [Authorizing access to health data](https://developer.apple.com/documentation/healthkit/authorizing-access-to-health-data)
- ESP32 : [HijelHID_BLEKeyboard](https://github.com/HijelHub/HijelHID_BLEKeyboard),
  [ESP32_BT_KB_IPAD_PIR](https://github.com/spelliott7/ESP32_BT_KB_IPAD_PIR),
  [esp32-iphone-agent](https://github.com/onehumanbeing/esp32-iphone-agent),
  [forum Apple 835175](https://developer.apple.com/forums/thread/835175)
- PC sans Mac : [SideTap](https://github.com/ucsandman/SideTap),
  [go-ios](https://github.com/danielpaulus/go-ios),
  [pymobiledevice3](https://github.com/doronz88/pymobiledevice3)
- Jailbreak : [Dopamine](https://github.com/opa334/Dopamine)
- WeWard : [FAQ – Valider mes pas](https://faq.weward.fr/hc/en-us/articles/28122448171794-Validate-my-steps-and-earn-rewards),
  [FAQ – pub à la conversion](https://faq.weward.fr/hc/en-gb/articles/9538963241618-A-video-ad-automatically-plays-when-I-convert-my-steps-Why),
  [Politique de confidentialité](https://www.wewardapp.com/legal/privacy-policy),
  [CGU](https://www.wewardapp.com/legal/terms-of-use)
