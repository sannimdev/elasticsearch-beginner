# Matching phrases

GET /recipe/default/\_search

-   순서가 매치 쿼리와 다르게 `match_phrase`에서는 중요하다
-   `"puttanesca spaghetti"`는 결괏값이 나오지 않음

```json
{
    "query": {
        "match_phrase": {
            "title": "spaghetti puttanesca"
        }
    }
}
```

```json
{
    "took": 1,
    "timed_out": false,
    "_shards": {
        "total": 1,
        "successful": 1,
        "skipped": 0,
        "failed": 0
    },
    "hits": {
        "total": {
            "value": 1,
            "relation": "eq"
        },
        "max_score": 3.5470161,
        "hits": [
            {
                "_index": "recipe",
                "_type": "_doc",
                "_id": "11",
                "_score": 3.5470161,
                "_ignored": ["description.keyword", "steps.keyword"],
                "_source": {
                    "title": "Spaghetti Puttanesca (Pasta or Spaghetti With Capers, Olives, and Anchovies)",
                    "description": "\"Puttanesca\" literally translates to \"in the style of prostitutes,\" supposedly because the pungent aromas of garlic, anchovies, capers, and olives tossed with pasta were how Neapolitan prostitutes would lead customers to their doors. This is one of those stories that seem, in the words of Douglas Adams, apocryphal or at least wildly inaccurate. That said, it's a fitting title — puttanesca packs an aromatic punch and then some.",
                    "preparation_time_minutes": 15,
                    "servings": {
                        "min": 2,
                        "max": 3
                    },
                    "steps": [
                        "Place spaghetti in a large skillet, sauté pan, or saucepan and cover with water. Add a small pinch of salt. Bring to a boil over high heat, stirring occasionally to prevent pasta from sticking.",
                        "Meanwhile, in a medium skillet, combine 4 tablespoons (60ml) oil, garlic, anchovies, and red pepper flakes. Cook over medium heat until garlic is very lightly golden, about 5 minutes. (Adjust heat as necessary to keep it gently sizzling.) Add capers and olives and stir to combine.",
                        "Add tomatoes, stir to combine, and bring to a bare simmer. Continue to simmer until pasta is cooked to just under al dente (about 1 minute less than the package recommends).",
                        "Using tongs, transfer pasta to sauce. Alternatively, drain pasta through a colander, reserving 1 cup of the cooking water. Add drained pasta to sauce.",
                        "Add a few tablespoons of pasta water to sauce and increase heat to bring pasta and sauce to a vigorous simmer. Cook, stirring and shaking the pan and adding more pasta water as necessary to keep sauce loose, until pasta is perfectly al dente, 1 to 2 minutes longer. (The pasta will cook more slowly in the sauce than it did in the water.) Stir in remaining olive oil, parsley, and cheese.",
                        "Season with salt and pepper. (Be generous with the pepper and scant with the salt — the dish will be plenty salty from the other ingredients.) If using, stir in canned tuna and break it up with a fork. Serve immediately with more grated cheese at the table."
                    ],
                    "ingredients": [
                        {
                            "name": "Dried spaghetti",
                            "amount": 225,
                            "unit": "grams"
                        },
                        {
                            "name": "Kosher salt"
                        },
                        {
                            "name": "Extra-virgin olive oil",
                            "amount": 6,
                            "unit": "tbsp"
                        },
                        {
                            "name": "Cloves garlic",
                            "amount": 4,
                            "unit": "pcs"
                        },
                        {
                            "name": "Anchovy fillets",
                            "amount": 5,
                            "unit": "pcs"
                        },
                        {
                            "name": "Large pinch red pepper flakes"
                        },
                        {
                            "name": "Capers",
                            "amount": 0.25,
                            "unit": "cups"
                        },
                        {
                            "name": "Pitted black olives",
                            "amount": 0.25,
                            "unit": "cups"
                        },
                        {
                            "name": "Whole peeled tomatoes",
                            "amount": 225,
                            "unit": "grams"
                        },
                        {
                            "name": "Minced fresh parsley leaves",
                            "amount": 1,
                            "unit": "handful"
                        },
                        {
                            "name": "Finely grated Pecorino Romano or Parmesan cheese",
                            "amount": 30,
                            "unit": "grams"
                        },
                        {
                            "name": "Freshly ground black pepper"
                        },
                        {
                            "name": "Can oil-packed tuna",
                            "amount": 140,
                            "unit": "grams"
                        }
                    ],
                    "created": "2011-02-21T11:01:26Z",
                    "ratings": [0.5, 1.0, 0.5, 1.5, 2.0]
                }
            }
        ]
    }
}
```
