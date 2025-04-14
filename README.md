# Nomeados ao Oscar

Contém a base de indicados ao Oscar em formato SQL para treinar comandos CRUD. 

Abaixo, algumas atividades para trabalharmos.

--- 

* Atualize os registros da tabela com os dados do Oscar 2025
  
R: Atualizei, e inseri no mongoDB
  
---

* Qual o **total** de registros na tabela indicados?

R: 11004

Q: 
```js
db.registros.countDocuments()
```
---

* Qual o número de indicações por categoria agrupadas por categoria?

R: "DIRECTING": 474, 
 <br>
 <br>
 "FILM EDITING": 450, 
 <br>
 <br>
 "ACTOR IN A SUPPORTING ROLE": 445, 
 <br>
 <br>
 "ACTRESS IN A SUPPORTING ROLE": 440,
 <br>
 <br>
 "BEST PICTURE": 381,
 <br>
 <br>
 "DOCUMENTARY (Short subject)": 378,
 <br>
 <br>
 "DOCUMENTARY (Feature)": 345,
 <br>
 <br>
 "CINEMATOGRAPHY": 338,
 <br>
 <br>
 "FOREIGN LANGUAGE FILM": 315, 
 <br>
 <br>
 "ART DIRECTION": 307,
 <br>
 <br>
 "COSTUME DESIGN": 295,
 <br>
 <br>
 "MUSIC (Original Score)": 270,
 <br>
 <br>
 "SOUND": 250, 
 <br>
 <br>
 "ACTOR IN  A LEADING ROLE": 245, 
 <br>
 <br>
 "ACTRESS IN A LEADING ROLE": 245,
 <br>
 <br>
 "ACTRESS": 236, 
 <br>
 <br>
 "MUSIC (Original Song)": 235, 
 <br>
 <br>
 "ACTOR": 232,
 <br>
 <br>
 "SHORT FILM (Live Action)": 226, 
 <br>
 <br>
 "SHORT FILM (Animated)" 215.

Q:
```js
db.registros.aggregate([
  {
    $group: {
      _id: "$categoria",  
      total_indicacoes: { $sum: 1 }  
    }
  },
  {
    $sort: { total_indicacoes: -1 } 
  }
]);
```

---

* Quantas vezes Natalie Portman foi indicada ao Oscar?

R: 3 vezes

Q:
```js
db.oscar.countDocuments({"Nome": "Natalie Portman"})
```

---

* Quantos Oscars Natalie Portman ganhou?

R: 1 oscar

Q:
```js
 db.registros.find({
"nome_do_indicado": "Natalie Portman", "vencedor": "true"
})
```
---

* Quantas vezes Viola Davis foi indicada ao Oscar?
  
R: 4 vezes

Q:
```js
db.registros.countDocuments({nome_do_indicado: "Viola Davis"})
```
---

* Quantos Oscars Viola Davis ganhou?

R: 1 vez

Q:
```js
db.registros.find({
nome_do_indicado: "Viola Davis", "vencedor": "true"})
```
---

* Amy Adams já ganhou algum Oscar?

R: nenhuma vez

Q:
```js
db.registros.find({
nome_do_indicado: "Amy Adams", "vencedor": "true"})
```
---

* Quais os atores/atrizes que foram indicados mais de uma vez?

R:1.440

Q: comando para saber quantos foram indicados mais de uma vez:
```js
 db.registros.aggregate([
  { $group: { _id: "$nome_do_indicado", total_indicacoes: { $sum: 1 } } },
  { $match: { total_indicacoes: { $gt: 1 } } },
  { $count: "quantidade_atores_mais_uma_indicacao" }
])
``` 

Q: Comando para saber nome e quantidade dos atores indicados mais de uma vez:
```js
 db.registros.aggregate([
  { $group: { _id: "$nome_do_indicado", total_indicacoes: { $sum: 1 } } },
  { $match: { total_indicacoes: { $gt: 1 } } }
])
``` 
---

* A série de filmes Toy Story ganhou Oscars em quais anos?

R: Duas vezes em 2011 e uma vez em 2020

Q:
```js
 db.registros.find({
nome_do_filme: /Toy Story/,
vencedor: "true"
})
``` 

---

* A partir de que ano que a categoria "Actress" deixa de existir?

R: Em 1928

Q:
```js
 db.registros.find({
categoria: "ACTRESS" ,
vencedor: "true"
}).sort({
ano_cerimonia: 1
}).limit(1)
``` 

---

* Quem ganhou o primeiro Oscar para Melhor Atriz? Em que ano?

R: Janet Gaynor no ano de 1928

Q:
```js
 db.registros.find({
categoria: "ACTRESS" ,
vencedor: "true"
}).sort({
ano_cerimonia: 1
})
``` 
---

* Na campo "Vencedor", altere todos os valores com "true" para 1 e todos os valores "false" para 0.

R: Dados atualizados

Q:
```js
db.registros.updateMany(
  { vencedor: "true" },
  { $set: { vencedor: 1 } }
);

db.registros.updateMany(
  { vencedor: "false" },
  { $set: { vencedor: 0 } }
);
``` 

---

* Em qual edição do Oscar "Crash" concorreu ao Oscar?

R: Na edição 78

Q:
```js
db.registros.find({
nome_do_filme: "Crash"
})
``` 

---

* O filme Central do Brasil aparece no Oscar?

R: Não aparece

Q:
```js
db.registros.find({
nome_do_filme: "Central do Brasil"
})
``` 

---

* Inclua no banco 3 filmes que nunca foram nem nomeados ao Oscar, mas que merecem ser.

R: Dados atualizados

Q:
```js
[
    {
        "id_registro": 1,
        "ano_filmagem": 2004,
        "ano_cerimonia": null,
        "cerimonia": null,
        "categoria": "MELHOR FILME DE ANIMAÇÃO",
        "nome_do_indicado": "Cory Edwards, Todd Edwards, Tony Leech",
        "nome_do_filme": "Deu a Louca na Chapeuzinho",
        "vencedor": false
    },
    {
        "id_registro": 2,
        "ano_filmagem": 2003,
        "ano_cerimonia": null,
        "cerimonia": null,
        "categoria": "MELHOR FILME DE COMEDIA",
        "nome_do_indicado": "Keenen Ivory Wayans",
        "nome_do_filme": "As Branquelas",
        "vencedor": false
    },
    {
        
        "id_registro": 3,
        "ano_filmagem": 1999,
        "ano_cerimonia": null,
        "cerimonia": null,
        "categoria": "MELHOR FILME DE ANIMAÇÃO",
        "nome_do_indicado": "Nick Park, Peter Lord",
        "nome_do_filme": "A Fuga das Galinhas",
        "vencedor": false   
    }
]
``` 

---

* Denzel Washington já ganhou algum Oscar?

R: Sim ele ganhou dois oscars

Q:
```js
db.registros.countDocuments({
"nome_do_indicado": "Denzel Washington",
vencedor: 1
});
``` 
---

* Quais os filmes que ganharam o Oscar de Melhor Filme?

R: 
Going My Way,
The Lost Weekend,
The Best Years of Our Lives,
Gentleman's Agreement,
Hamlet,
All the King's Men,
All about Eve,
An American in Paris,
The Greatest Show on Earth,
From Here to Eternity,
On the Waterfront,
Marty,
Around the World in 80 Days,
The Bridge on the River Kwai,
Gigi,
Ben-Hur,
The Apartment,
West Side Story.

Q:
```js
db.registros.find(
  {
    categoria: "BEST MOTION PICTURE", 
    vencedor: 1 
  },
  {
    nome_do_filme: 1, 
    _id: 0 
  }
)
``` 
---

* Sidney Poitier foi o primeiro ator negro a ser indicado ao Oscar. Em que ano ele foi indicado? Por qual filme?

R: Ele foi indicado primeiramente em 1959 no filme The Defiant Ones

Q:
```js
 db.registros.find({
nome_do_indicado: "Sidney Poitier"
})
``` 
---

* Quais os filmes que ganharam o Oscar de Melhor Filme e Melhor Diretor na mesma cerimonia?

R: Gentleman's Agreement,
Marty,
The Lost Weekend,
The Bridge on the River Kwai,
The Apartment,
From Here to Eternity,
All about Eve,
The Best Years of Our Lives,
West Side Story,
Ben-Hur,
On the Waterfront,
Gigi,
Going My Way

Q:
```js
db.registros.aggregate(
    { $match: { categoria: { $in: ["BEST MOTION PICTURE", "DIRECTING"] }, vencedor: 1 } },
    { $group: { _id: { cerimonia: "$cerimonia", filme: "$nome_do_filme" }, categorias: { $addToSet: "$categoria" } } },
    { $match: { categorias: { $all: ["BEST MOTION PICTURE", "DIRECTING"] } } },
);
``` 
---

* Denzel Washington e Jamie Foxx já concorreram ao Oscar no mesmo ano?

R: Os dois não concorreram ao oscar no mesmo ano segundo o banco de dados

Q:

```js
db.backup.find({ nome_do_indicado: "Denzel Washington" })
db.backup.find({ nome_do_indicado: "Jamie Foxx" })
```

```js
db.registros.aggregate([
  {
    $match: {
      nome_do_indicado: { $in: ["Denzel Washington", "Jamie Foxx"] }
    }
  },
  {
    $group: {
      _id: "$ano_cerimonia",
      indicados: { $addToSet: "$nome_do_indicado" }
    }
  },
  {
    $match: {
      indicados: { $all: ["Denzel Washington", "Jamie Foxx"] }
    }
  },
  {
    $project: {
      _id: 0,
      ano_cerimonia: "$_id",
      indicados: 1
    }
  }
]);
```
