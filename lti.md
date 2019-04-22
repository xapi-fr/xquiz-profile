# Profil xQuiz : Intégration LTI

---

- [Contexte](#context)
- [Prérequis](#requirements)
- [Accès au service](#access)
- [Données entrantes](#input)
- [Données sortantes](#output)


<a name="context"></a>
## Contexte

Lorsqu'une application LTI veut envoyer des Statements au LRS, il lui manque généralement l'identification xAPI de certains éléments du Statement ainsi que certaines informations de contexte. Dans le cas d'une intégration xQuiz / Moodle, il manque :

- Identification (anonymisée) de l'acteur
- Identification de l'activité LTI dans Moodle
- Identification du cours Moodle contenant l'activité LTI
- Identification de l'instance Moodle contenant l'activité LTI
- Identification de l'instructeur dans certains cas (spécifique xQuiz)

Pour autant, l'activité LTI dispose de certaines informations précieuses. Dans le cas de Moodle :

- `user_id` : identifiant interne Moodle de l'utilisateur
- `resource_link_id` : identifiant interne Moodle de l'activité LTI
- `context_id` : identifiant interne Moodle du cours contenant l'activité LTI

Pour pouvoir générer des données xAPI à partir de ces informations, un service Web a été implanté au niveau de Moodle, dans le plugin `TRAX Logs for Moodle`.


<a name="requirements"></a>
## Prérequis

### TRAX Logs for Moodle

- Utiliser la dernière version du plugin, directement accessible depuis Github : https://github.com/trax-project/moodle-trax-logs.

- Installer le plugin, le paramétrer et l'activer (Administration > Plugins > Logging > Trax Logs).

- Naviguer dans un cours, puis une activité LTI afin de vérifier que les Statements sont bien générés et envoyés au LRS.

### Moodle 3.5

- Utiliser Moodle 3.5 avec TRAX Logs installé au préalable.

- Activer et configurer les Web Services en suivant les instructions (Administration > Plugins > Web Services > Overview).

- Activer de préférence le protocole REST, qui sera utilisé pour tester le fonctionnement du service.

- Au moment de choisir les fonctions, sélectionner `logstore_trax_get_activities` et `logstore_trax_get_actors`. 

- Ne pas oublier de créer un utilisateur autorisé et de lui générer un jeton.

- Pour tester le service, utiliser les 2 scripts PHP situés dans `/admin/tool/log/store/trax/tests/client`, à savoir `get_activities` et `get_actors`, après les avoir adapté (cf commentaires à l'intérieur de ces scripts).


<a name="access"></a>
## Accès au service

- Méthode : POST

- Endpoint : `http://my-moodle.com/webservice/rest/server.php?moodlewsrestformat=json&wsfunction=xxx&wstoken=yyy` où `xxx` est le nom de la fonction, à savoir `logstore_trax_get_activities` ou `logstore_trax_get_actors`, et `yyy` le numéro de token généré dans Moodle.


<a name="input"></a>
## Données entrantes

Un objet avec une propriété `items` contenant une liste d'objets avec pour chacun d'eux un `type` et un  `id` Moodle.

### Exemple de demande d'activités

Cet exemple permet de récupérer les 3 activités à utiliser dans le contexte des Statements : l'instance Moodle (system), le cours Moodle (course) et l'activité LTI (lti).

``` json
{
    "items": [
        {
            "type": "system",
            "id": 0
        },
        {
            "type": "course",
            "id": 2
        },
        {
            "type": "lti",
            "id": 1
        }
    ]
}
```

### Exemple de demande d'acteurs

Cet exemple permet de récupérer 2 acteurs, par exemple l'apprenant et le formateur ayant lancé la session xQuiz :

``` json
{
    "items": [
        {
            "type": "user",
            "id": 2
        },
        {
            "type": "user",
            "id": 10
        }
    ]
}
```

<a name="output"></a>
## Données sortantes

Une liste d'objets avec pour chacun d'eux un `type` et un  `id` Moodle (les mêmes que ceux fournis en entrée), ainsi qu'une propriété `xapi` contenant la chaîne JSON xAPI encodée.

### Exemple de récupération d'activités

``` json
[
    {
        "type": "system",
        "id": 0,
        "xapi": "{\"objectType\":\"Activity\",\"id\":\"http:\\\/\\\/xapi.moodle.test\\\/xapi\\\/activities\\\/system\",\"definition\":{\"type\":\"http:\\\/\\\/vocab.xapi.fr\\\/activities\\\/system\"}}"
    },
    {
        "type": "course",
        "id": 2,
        "xapi": "{\"objectType\":\"Activity\",\"id\":\"http:\\\/\\\/xapi.moodle.test\\\/xapi\\\/activities\\\/course\\\/8acfd7a3-2490-40c8-9b61-ec65d518f7da\",\"definition\":{\"type\":\"http:\\\/\\\/vocab.xapi.fr\\\/activities\\\/course\"}}"
    },
    {
        "type": "lti",
        "id": 1,
        "xapi": "{\"objectType\":\"Activity\",\"id\":\"http:\\\/\\\/xapi.moodle.test\\\/xapi\\\/activities\\\/lti\\\/e403e7ee-4cdd-4d25-b7d9-5de3569a1cc2\",\"definition\":{\"type\":\"http:\\\/\\\/vocab.xapi.fr\\\/activities\\\/external-activity\"}}"
    }
]
```

### Exemple de récupération d'acteurs


``` json
[
    {
        "type": "user",
        "id": 2,
        "xapi": "{\"objectType\":\"Agent\",\"account\":{\"homePage\":\"http:\\\/\\\/xapi.moodle.test\",\"name\":\"23a5bb2e-80c5-464a-8472-632261df912d\"}}"
    },
    {
        "type": "user",
        "id": 10,
        "xapi": "{\"objectType\":\"Agent\",\"account\":{\"homePage\":\"http:\\\/\\\/xapi.moodle.test\",\"name\":\"564642e-80c5-464a-8472-632264564564\"}}"
    }
]
```

