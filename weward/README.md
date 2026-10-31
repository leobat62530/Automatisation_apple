# WeWard : ouverture automatique à 22h30

## Ce que l'iPhone peut faire (et ne peut pas faire)

| Action | Avec Raccourcis ? |
|---|---|
| Ouvrir WeWard tous les jours à 22h30, sans rien toucher | ✅ Oui |
| T'envoyer une notification de rappel | ✅ Oui |
| Appuyer sur « Valider mes pas » | ❌ Non |
| Fermer une pub ou une pop-up | ❌ Non |

Sur iOS, un raccourci ne peut pas appuyer sur les boutons d'une autre app. Il
peut seulement utiliser les actions que cette app propose dans Raccourcis.

**Vérifie d'abord** : dans Raccourcis, appuie sur « + » puis « Ajouter une
action » et tape `WeWard` dans la recherche.
- Si une action comme « Valider mes pas » apparaît, ajoute-la après « Ouvrir
  l'app » : la validation sera alors entièrement automatique.
- Si aucune action n'apparaît, WeWard ne propose rien et il faudra appuyer toi-même
  sur « Valider mes pas ».

## Créer l'automatisation (2 minutes)

1. Ouvre l'app **Raccourcis**, puis l'onglet **Automatisation**.
2. Appuie sur **+** (ou « Nouvelle automatisation »), puis sur **Heure de la journée**.
3. Règle :
   - **Heure** : `22:30`
   - **Répéter** : `Quotidien`
   - Coche **Exécuter immédiatement**
   - Désactive « Avertir lors de l'exécution » si tu veux qu'elle se lance sans bruit
4. Appuie sur **Suivant**, puis sur **Nouveau raccourci vide**.
5. Ajoute ces actions dans cet ordre :
   1. **Afficher la notification** avec le texte
      `👟 Valide tes pas WeWard avant minuit !`
   2. **Ouvrir l'app**, puis choisis **WeWard**
6. Appuie sur **OK**.

Pour tester sans attendre 22h30, ouvre l'automatisation et appuie sur ▶︎.

### Filet de sécurité (optionnel)

Crée une deuxième automatisation identique à **23h30**. Si tu as raté la
première, tu as encore 30 minutes pour valider.

## Bon à savoir

- **iPhone verrouillé à 22h30** : iOS ne peut pas afficher une app par-dessus
  l'écran verrouillé. Tu reçois quand même la notification, et il te suffit de
  déverrouiller ton iPhone et d'ouvrir WeWard.
- **Ouvrir l'app suffit-il ?** Les sources ne sont pas d'accord : certaines
  disent qu'ouvrir l'app suffit, d'autres qu'il faut appuyer sur « Valider mes
  pas » avant minuit. Pour en avoir le cœur net, laisse l'automatisation ouvrir
  WeWard un soir sans rien toucher, et regarde le lendemain si les Wards de la
  veille ont été crédités.
- **Pubs et pop-ups** : Raccourcis ne peut pas les fermer. Tu dois appuyer
  toi-même sur la croix ou sur « Passer ».

## Et pour tout automatiser, appuis compris ?

C'est techniquement possible, mais seulement depuis un **Mac** qui pilote
l'iPhone (Xcode + Appium/WebDriverAgent). Le script lirait l'écran, appuierait sur
« Valider mes pas » et fermerait les pop-ups. Les limites :

- le Mac doit être allumé à 22h30, et l'iPhone branché ou sur le même Wi-Fi et
  déverrouillé ;
- avec un compte développeur Apple gratuit, l'outil doit être réinstallé tous
  les 7 jours ;
- le script casse dès que WeWard modifie son interface ;
- l'automatisation des appuis est sans doute contraire aux conditions
  d'utilisation de WeWard (vérifie leurs CGU), avec un risque de suspension du
  compte et de perte des Wards.

L'automatisation Raccourcis ci-dessus plus un appui sur le bouton reste la
solution fiable.
