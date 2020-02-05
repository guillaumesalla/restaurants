1. Écrire la requête qui retourne tous les documents dans la collection "restaurants"

db.restaurants.find({})

2. Retourner les champs restaurant_id, name, borough et cuisine pour tous les documents de la collection

db.restaurants.find({},{'restautant_id': 1, 'name': 1, 'borough':1, 'cuisine': 1})

3. Retourner les champs restaurant_id, name, borough et cuisine pour tous les documents de la collection, cette fois-ci, sans afficher le champs _id

db.restaurants.find({}, {restaurant_id: 1, name: 1, borough: 1, cuisine: 1, _id: 0})

4. Retourner les champs restaurant_id, name, borough, cuisine et zip_code de tous les documents, sans le champs _id

db.restaurants.find({}, { _id:0, restaurant_id:1, name:1, borough:1, cuisine:1 , zip_code: 1})

5. Retourner tous les restaurants qui sont dans le Bronx

db.restaurants.find({'borough':'Bronx'}, {})

6. Retourner les 5 premiers restaurants qui sont dans le Bronx

db.restaurants.find({borough: 'Bronx'}).limit(5)

7. Retourner les 5 suivants

db.restaurants.find({borough:'Bronx'}).limit(10)

8. Retourner les restaurants avec un score supérieur à 90

db.restaurants.find({'grades.score': {$gt: 90}})

9. Retourner les restaurants avec un score supérieur à 80 et en-dessous de 100

db.restaurants.find({"grades.score": {$gt: 90, $lt: 100}})

10. Retourner les restaurants situés à une latitude inférieure à -95.754168

db.restaurants.find({'address.coord.0': {$lt: -95.754168}})

11. Retourner les restaurants qui (sans utiliser l'opérateur $and) : 
    - Ne préparent pas de cuisine 'American' 
    - Ont un score au dessus de 70 7
    - Une latitude inférieure à -65.754168

db.restaurants.find({cuisine: {$ne:'American'}, "grades.score": {$gt: 70}, "address.coord.0": {$lt: -65.754168 }})

12. Même requête avec l'opérateur $and

db.restaurants.find({$and:[{'cuisine':{$ne:'American'}},{'grades.score':{$gt:70}},{'address.coord':{$lt:-65.754168}}]})

13. Retouner, ordonné par type de cuisine, les restaurants qui : 
    - Ne préparent pas de cuisine 'American' 
    - Ont une grade "A" 
    - En dehors de "Brooklyn"

db.restaurants.find({ cuisine: {$ne:'American '}, 'grades.grade': 'A', borough: {$ne:'Brooklyn'} }).sort({cuisine: 1})

14. Retourner l'id, le nom, le quartier et la cuisine des resraurants dont le nom commence par "Wil"

db.restaurants.find({name:{$in:[ /^Wil/ ]}}, {restaurant_id: 1, name: 1, borough: 1, cuisine: 1})

15. Retourner l'id, le nom, le quartier et la cuisine des resraurants dont le nom termine par "ces"

db.restaurants.find({ $and:[{'name': /ces$/}]}, {'_id': false, 'restaurant_id': true, 'name': true,  'borough': true, 'cuisine': true})

16. Retourner l'id, le nom, le quartier et la cuisine des resraurants dont le nom contient "Reg"

db.restaurants.find({name: {$regex: "Reg"}}, {_id: 1, name: 1, "address.building": 1, "address.street": 1, "address.zipcode": 1, cuisine: 1})

17. Retourner les restaurants du Bronx qui servent de la cuisine américaine ou chinoise

db.restaurants.find({'borough':'Bronx',$or:[{'cuisine':'American'},{'cuisine':'Chinese'}]})

18. Retourner l'id, le nom, le quartier et la cuisine qui ne se situent à Staten Island ou à Brooklyn ou dans le Bronx

db.restaurants.find({borough: {$in: ["Staten Island", "Broolyn", "Bronx"]}}, {restaurant_id: 1, name: 1, borough: 1, cuisine: 1})

19. Retourner l'id, le nom, le quartier et la cuisine qui ne se situent ni à Staten Island, ni à Brooklyn et ni dans le Bronx

db.restaurants.find({borough: {$nin: ['Staten Island', 'Brooklyn', 'Bronx']}}, {restaurant_id: 1, name: 1, borough: 1, cuisine: 1})

20. Retourner l'id, le nom, le quartier et la cuisine des resraurants avec un score inférieur à 10

db.restaurants.find({'grades.score': {$lt:10}}, {'restaurant_id': true, 'name': true, 'borough': true, 'cuisine': true})

21. Retourner l'id, le nom, le quartier et la cuisine des resraurants qui ne sont ni Chinois ou Américain; ou bien dont le nom commence par "Wil"

db.restaurants.find({$or: [{name:{$regex: "^Wil"}}, {cuisine:{$nin: ["American ", "Chinese"]}}]}, {_id: 1, name: 1, "address.building": 1, "address.street": 1, "address.zipcode": 1, cuisine: 1})


22. Retourner l'id, le nom et les grades des restaurants possédant une grade avec les valeurs: 
    - Une grade "A" 
    - Un score 11 s 
    - "2014-08-11T00:00:00Z"

db.restaurants.find({'grades.date':ISODate('2014-08-11T00:00:00Z'),'grades.grade':'A','grades.score':11},{'restaurant_id':1,'name':1,'grades':1})

23. Retourner l'id, le nom et les grades des restaurants dont 2eme élément de grades possède: 
    - Une grade "A" 
    - Un score 9 
    - Une ISODate "2014-08-11T00:00:00Z"

db.restaurants.find({ "grades.1.grade": "A", "grades.1.score": 9, "grades.1.date": ISODate("2014-08-11T00:00:00.000Z")}, {restaurant_id: 1, name: 1, grades: 1})

24. Retourner l'id, le nom et la position géographique dont le deuxième élément des coordonnées géographiques contient une valeur entre 42 et 52

db.restaurants.find({"address.coord.1": { $gt:42, $lte:52 }}, {restaurant_id:1, name:1, "address.coord": 1 })

25. Ajoute tes 5 restaurants préférés en remplissant pour chacun : 
    - Une adresse complète 
    - Un quartier 
    - Un type de cuisine 
    - 3 notes au minimum 
    - Un nom 
    - Un id de restaurant (Pas l'_id mongo !)

db.restaurants.insertMany([
    { 
        address: { 
            building: 10, 
            coord: [47.155,-0.568954], 
            street: 'quai des chartrons', 
            zipcode: 333000
        },
        borough: 'chartrons',
        cuisine: 'Orientale',
        grades: [
            {
                date: ISODate("2014-10-08T00:00:00.000Z"),
                grade: 'A',
                score: 100
            },
            {
                date: ISODate("2016-05-24T00:00:00.000Z"),
                grade: 'B',
                score: 50
            },
            {
                date: ISODate("2019-01-19T00:00:00.000Z"),
                grade: 'C',
                score: 25
            }
        ],
        name: 'Couscous',
        restaurant_id: 3300001
    },
    { 
        address: { 
            building: 94, 
            coord: [74.42664,-0.41554894], 
            street: 'avenue de navarre', 
            zipcode: 64100
        },
        borough: 'Navarre',
        cuisine: 'Francaise',
        grades: [
            {
                date: ISODate("2019-09-11T00:00:00.000Z"),
                grade: 'A',
                score: 100
            },
            {
                date: ISODate("2018-02-17T00:00:00.000Z"),
                grade: 'B',
                score: 50
            },
            {
                date: ISODate("2018-01-23T00:00:00.000Z"),
                grade: 'C',
                score: 25
            }
        ],
        name: 'FrenchResto',
        restaurant_id: 6410001
    },
    { 
        address: { 
            building: 29, 
            coord: [44.4848455,-0.524989], 
            street: 'rue du cafe', 
            zipcode: 33000
        },
        borough: 'cafe',
        cuisine: 'Vietnam',
        grades: [
            {
                date: ISODate("2019-04-17T00:00:00.000Z"),
                grade: 'A',
                score: 100
            },
            {
                date: ISODate("2014-12-17T00:00:00.000Z"),
                grade: 'B',
                score: 50
            },
            {
                date: ISODate("2015-11-19T00:00:00.000Z"),
                grade: 'C',
                score: 25
            }
        ],
        name: 'CafeViet',
        restaurant_id: 3300002
    },
    { 
        address: { 
            building: 54, 
            coord: [44.787446,-0.5801549], 
            street: 'Rue Sainte-Catherine', 
            zipcode: 33000
        },
        borough: 'Sainte-Catherine',
        cuisine: 'Hawaïen',
        grades: [
            {
                date: ISODate("2019-07-17T00:00:00.000Z"),
                grade: 'A',
                score: 100
            },
            {
                date: ISODate("2017-03-04T00:00:00.000Z"),
                grade: 'B',
                score: 50
            },
            {
                date: ISODate("2012-10-06T00:00:00.000Z"),
                grade: 'C',
                score: 25
            }
        ],
        name: 'Coolos',
        restaurant_id: 3300003
    },
    { 
        address: { 
            building: 45, 
            coord: [45.005656,-0.5698765], 
            street: 'Rue du freestyle', 
            zipcode: 64100
        },
        borough: 'PawPaw',
        cuisine: 'Kebab',
        grades: [
            {
                date: ISODate("2018-02-01T00:00:00.000Z"),
                grade: 'A',
                score: 100
            },
            {
                date: ISODate("2014-07-14T00:00:00.000Z"),
                grade: 'B',
                score: 50
            },
            {
                date: ISODate("2018-03-27T00:00:00.000Z"),
                grade: 'C',
                score: 25
            }
        ],
        name: 'FreestyleKebab',
        restaurant_id: 6410002
    },
])

26. Ajoute 2 notes à chacun de ces restaurants

db.restaurants.updateMany(
    {$and: [
        {"restaurant_id": {$gte: 3300001}},
        {"restaurant_id": {$lte: 3300002}},
        {"restaurant_id": {$lte: 3300003}},
        {"restaurant_id": {$lte: 6410001}},
        {"restaurant_id": {$lte: 6410002}}
    ]},
    {$push: {"grades": {$each: [
        {'date': '2019-05-14 00:00:00.000Z', 'grade': 'A', 'score': 100},
        {'date': '2018-11-04 00:00:00.000Z', 'grade': 'A', 'score': 50},
    ]}}}``

27. Supprime de la base les restaurants de Staten Island

db.restaurants.remove({'borough':'Staten Island'})

28. Transfert tous les restaurants du Manhattan à Brooklyn (Pour info, appelle ça la gentrification)

db.restaurants.update({borough: "Manhattan"}, {$set: {borough: "Brooklyn"}}, {multi: true})

29. Retourne la liste de tous les quartiers de la collection (Si tu as encore des restaurants dans Manhattan, retourne à la question 28 )

db.restaurants.find({}, {_id: 0, borough: 1})

