# Profil xQuiz : Identification

---

- [Agents](#agents)
- [Activités](#activities)
- [Profil](#profile)


<a name="agents"></a>
## Agents

### Identification en clair

Les agents peuvent être identifiés en clair de manière provisoire, à titre de démonstration. On s’appuie sur les identifiants remontés par Moodle dans le cadre de l’intégration LTI avec xQuiz.

``` json
"actor": {
    "objectType": "Agent",
    "name": "Jon Snow",
    "account": {
        "homePage": "http://moodle.isae.fr",
        "name": "jon.snow"
    }
}
```

### Identification anonymisée

En production, on s’appuie sur des identifiants anonymisés, gérés de manière centralisée par un service de gestion des identités (potentiellement le LDAP).

``` json
"actor": {
    "objectType": "Agent",
    "account": {
        "homePage": "http://moodle.isae.fr",
        "name": "8be985de-9d4f-331a-8f06-fd4bab2030f7"
    }
}
```

<a name="activities"></a>
## Activités

- Les sessions xQuiz sont identifiées selon le schéma `http://platform_iri/xapi/activities/session/xxx`.

- Les questions xQuiz sont identifiées selon le schéma `http://platform_iri/xapi/activities/session/xxx/question/yyy`.


<a name="profile"></a>
## Profils

- Le profil xQuiz est identifié par `http://xapi.isae.fr/vocab/profiles/xquiz`.

- Le profil VLE est identifié par `http://vocab.xapi.fr/categories/vle-profile`.



