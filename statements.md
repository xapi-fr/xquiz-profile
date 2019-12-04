# Profil xQuiz : Statements

---

- [Règles communes](#rules)
- [T01 - L’enseignant a créé une session.](#T01)
- [T02 - L’enseignant a posé une question.](#T02)
- [L01 - L’apprenant a répondu à une question.](#L01)
- [L02 - L’apprenant s’est déclaré perdu.](#L02)


<a name="rules"></a>
### Règles communes

- Le `type` des activités DOIT être précisé.
- Le profil xQuiz DOIT être indiqué dans la propriété `context.contextActivities.category`. 
- Le profil VLE DOIT être indiqué dans la propriété `context.contextActivities.category`. 
- Lorsque xQuiz est lancé depuis Moodle, via une activité LTI, la propriété `context.contextActivities.grouping` DOIT mentionner l'instance de la plateforme Moodle, ainsi que le cours et l'activité LTI concernés. 
- La propriété `context.platform` DOIT être précisée et avoir pour valeur `xQuiz`.
- La propriété `timestamp` DOIT être précisée.


<a name="T01"></a>
### T01 - L’enseignant a créé une session.

- L’extension d’activité `learning-outcomes` PEUT être utilisée pour préciser les objectifs pédagogiques, sous forme d'une liste de tags.

``` json
{
    "actor": {
        "objectType": "Agent",
        "account": {
            "homePage": "http://moodle.isae.fr",
            "name": "john.snow"
        }
    },
    "verb": {
        "id": "http://adlnet.gov/expapi/verbs/initialized"
    },
    "object": {
        "objectType": "Activity",
        "id": "http://xquiz.isae.fr/xapi/activities/session/123456",
        "definition": {
            "type": "http://vocab.xapi.fr/activities/live-session",
            "extensions": {
                "http://vocab.xapi.fr/extensions/learning-outcomes": ["tag1", "tag2"]
            }
        }
    },
    "context": {
        "contextActivities": {
            "grouping": [
                {
                    "id": "http://xapi.moodle.test",
                    "definition": {
                        "type": "http://vocab.xapi.fr/activities/system"
                    }
                },
                {
                    "id": "http://xapi.moodle.test/xapi/activities/course/ba297687-b1aa-4477-9efd-a782c8fdb90a",
                    "definition": {
                        "type": "http://vocab.xapi.fr/activities/course"
                    }
                },
                {
                    "id": "http://xapi.moodle.test/xapi/activities/lti/e403e7ee-4cdd-4d25-b7d9-5de3569a1cc2",
                    "definition": {
                        "type": "http://vocab.xapi.fr/activities/external-activity"
                    }
                }
            ],            
            "category": [
                {
                    "id": "http://xapi.isae.fr/vocab/profiles/xquiz",
                    "definition": {
                        "type": "http://adlnet.gov/expapi/activities/profile"
                    }
                },
                {
                    "id": "http://vocab.xapi.fr/categories/vle-profile",
                    "definition": {
                        "type": "http://adlnet.gov/expapi/activities/profile"
                    }
                }
            ]
        },
        "platform": "xQuiz"
    },
    "timestamp": "2019-02-25T15:34:25.804+00:00"
}

```


<a name="T02"></a>
### T02 - L’enseignant a posé une question.

- La propriété `object.definition.name` DOIT indiquer le numéro de la question.
- La propriété `object.definition.description` PEUT être utilisée pour indiquer l’intitulé de la question.
- L'extension d'activité `number-of-options` DOIT indiquer le nombre de réponses possibles.
- L’extension d’activité `question-type` PEUT être utilisée pour mentionner le type de question, à savoir `choice` (question fermée) ou `fill-in` (question ouverte).
- L’extension d’activité `correct-responses` PEUT être utilisée pour préciser les numéros ou intitulés des réponses correctes, sous forme de liste.
- L’extension d’activité `learning-outcomes` PEUT être utilisée pour préciser les objectifs pédagogiques, sous forme d'une liste de tags.
- L’extension de contexte `question-mode` DOIT indiquer le mode de questionnement, à savoir `normal` ou `debate`.
- La session xQuiz DOIT être indiquée dans la propriété `context.contextActivities.parent`. 

``` json
{
    "actor": {
        "objectType": "Agent",
        "account": {
            "homePage": "http://moodle.isae.fr",
            "name": "john.snow"
        }
    },
    "verb": {
        "id": "http://adlnet.gov/expapi/verbs/asked"
    },
    "object": {
        "objectType": "Activity",
        "id": "http://xquiz.isae.fr/xapi/activities/session/123456/question/001",
        "definition": {
            "type": "http://adlnet.gov/expapi/activities/question",
            "name": {
                "fr-FR": "Numero de la question"
            },
            "description": {
                "fr-FR": "Intitulé de la question si fourni"
            },
            "extensions": {
                "http://vocab.xapi.fr/extensions/number-of-options": 3,
                "http://vocab.xapi.fr/extensions/question-type": "choice",
                "http://vocab.xapi.fr/extensions/correct-responses": ["2", "3"],
                "http://vocab.xapi.fr/extensions/learning-outcomes": ["tag1", "tag2"]
            }
        }
    },
    "context": {
        "contextActivities": {
            "parent": [
                {
                    "id": "http://xquiz.isae.fr/xapi/activities/session/123456",
                    "definition": {
                        "type": "http://vocab.xapi.fr/activities/live-session"
                    }
                }
            ],
            "grouping": [
                {
                    "id": "http://xapi.moodle.test",
                    "definition": {
                        "type": "http://vocab.xapi.fr/activities/system"
                    }
                },
                {
                    "id": "http://xapi.moodle.test/xapi/activities/course/ba297687-b1aa-4477-9efd-a782c8fdb90a",
                    "definition": {
                        "type": "http://vocab.xapi.fr/activities/course"
                    }
                },
                {
                    "id": "http://xapi.moodle.test/xapi/activities/lti/e403e7ee-4cdd-4d25-b7d9-5de3569a1cc2",
                    "definition": {
                        "type": "http://vocab.xapi.fr/activities/external-activity"
                }
            ],            
            "category": [
                {
                    "id": "http://xapi.isae.fr/vocab/profiles/xquiz",
                    "definition": {
                        "type": "http://adlnet.gov/expapi/activities/profile"
                    }
                },
                {
                    "id": "http://vocab.xapi.fr/categories/vle-profile",
                    "definition": {
                        "type": "http://adlnet.gov/expapi/activities/profile"
                    }
                }
            ]
        },
        "platform": "xQuiz",
        "extensions": {
            "http://vocab.xapi.fr/extensions/question-mode": "debate"
        }
    },
    "timestamp": "2019-02-25T15:34:25.804+00:00"
}

```


<a name="L01"></a>
### L01 - L’apprenant a répondu à une question.

- La propriété `result.score` PEUT être renseignée (questions fermées dont les bonnes réponses sont connues). Lorsqu’elle est renseignée, les propriétés `raw`, `min`, `max` et `scaled` DOIVENT être précisées.
- L’extension de résultat `responses` DOIT préciser les numéros ou intitulés des réponses fournies, sous forme de liste.
- La session xQuiz DOIT être indiquée dans la propriété `context.contextActivities.parent`. 
- L’extension de contexte `question-mode` DOIT indiquer le mode de questionnement, à savoir `normal` ou `debate`.
- La propriété `context.instructor` DOIT être présente et préciser l'enseignant qui a posé la question concerné.

``` json
{
    "actor": {
        "objectType": "Agent",
        "account": {
            "homePage": "http://moodle.isae.fr",
            "name": "arya.stark"
        }
    },
    "verb": {
        "id": "http://adlnet.gov/expapi/verbs/answered"
    },
    "object": {
        "objectType": "Activity",
        "id": "http://xquiz.isae.fr/xapi/activities/session/123456/question/001",
        "definition": {
            "type": "http://adlnet.gov/expapi/activities/question"
        }
    },
    "result": {
        "score": {
            "raw": 1,
            "min": 0,
            "max": 3,
            "scaled": 0.33
        },
        "extensions": {
            "http://vocab.xapi.fr/extensions/responses": ["1", "2"]
        }
    },
    "context": {
        "contextActivities": {
            "parent": [
                {
                    "id": "http://xquiz.isae.fr/xapi/activities/session/123456",
                    "definition": {
                        "type": "http://vocab.xapi.fr/activities/live-session"
                    }
                }
            ],
            "grouping": [
                {
                    "id": "http://xapi.moodle.test",
                    "definition": {
                        "type": "http://vocab.xapi.fr/activities/system"
                    }
                },
                {
                    "id": "http://xapi.moodle.test/xapi/activities/course/ba297687-b1aa-4477-9efd-a782c8fdb90a",
                    "definition": {
                        "type": "http://vocab.xapi.fr/activities/course"
                    }
                },
                {
                    "id": "http://xapi.moodle.test/xapi/activities/lti/e403e7ee-4cdd-4d25-b7d9-5de3569a1cc2",
                    "definition": {
                        "type": "http://vocab.xapi.fr/activities/external-activity"
                }
            ],            
            "category": [
                {
                    "id": "http://xapi.isae.fr/vocab/profiles/xquiz",
                    "definition": {
                        "type": "http://adlnet.gov/expapi/activities/profile"
                    }
                },
                {
                    "id": "http://vocab.xapi.fr/categories/vle-profile",
                    "definition": {
                        "type": "http://adlnet.gov/expapi/activities/profile"
                    }
                }
            ]
        },
        "instructor": {
            "objectType": "Agent",
            "account": {
                "name": "b1f6a84b-de23-3e28-9f34-48344e2f20df",
                "homePage": "http://moodle.isae.fr"
            }
        },
        "platform": "xQuiz",
        "extensions": {
            "http://vocab.xapi.fr/extensions/question-mode": "debate"
        }
    },
    "timestamp": "2019-02-25T15:34:25.804+00:00"
}

```


<a name="L02"></a>
### L02 - L’apprenant s’est déclaré perdu.

- La session xQuiz DOIT être indiquée dans la propriété `context.contextActivities.parent`. 
- La propriété `context.instructor` DOIT être présente et préciser l'enseignant qui a posé la question concerné.

``` json
{
    "actor": {
        "objectType": "Agent",
        "account": {
            "homePage": "http://moodle.isae.fr",
            "name": "arya.stark"
        }
    },
    "verb": {
        "id": "http://id.tincanapi.com/verb/requested-attention"
    },
    "object": {
        "objectType": "Activity",
        "id": "http://xquiz.isae.fr/xapi/activities/session/123456",
        "definition": {
            "type": "http://vocab.xapi.fr/activities/live-session"
        }
    },
    "context": {
        "contextActivities": {
            "parent": [
                {
                    "id": "http://xquiz.isae.fr/xapi/activities/session/123456",
                    "definition": {
                        "type": "http://vocab.xapi.fr/activities/live-session"
                    }
                }
            ],
            "grouping": [
                {
                    "id": "http://xapi.moodle.test",
                    "definition": {
                        "type": "http://vocab.xapi.fr/activities/system"
                    }
                },
                {
                    "id": "http://xapi.moodle.test/xapi/activities/course/ba297687-b1aa-4477-9efd-a782c8fdb90a",
                    "definition": {
                        "type": "http://vocab.xapi.fr/activities/course"
                    }
                },
                {
                    "id": "http://xapi.moodle.test/xapi/activities/lti/e403e7ee-4cdd-4d25-b7d9-5de3569a1cc2",
                    "definition": {
                        "type": "http://vocab.xapi.fr/activities/external-activity"
                }
            ],            
            "category": [
                {
                    "id": "http://xapi.isae.fr/vocab/profiles/xquiz",
                    "definition": {
                        "type": "http://adlnet.gov/expapi/activities/profile"
                    }
                },
                {
                    "id": "http://vocab.xapi.fr/categories/vle-profile",
                    "definition": {
                        "type": "http://adlnet.gov/expapi/activities/profile"
                    }
                }
            ]
        },
        "instructor": {
            "objectType": "Agent",
            "account": {
                "name": "b1f6a84b-de23-3e28-9f34-48344e2f20df",
                "homePage": "http://moodle.isae.fr"
            }
        },
        "platform": "xQuiz"
    },
    "timestamp": "2019-02-25T15:34:25.804+00:00"
}
```


