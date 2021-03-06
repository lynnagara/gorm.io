---
title: Belongs To
layout: page
---
## Belongs To

Eine `belongs to` Assoziation stellt eine Eins-zu-Eins-Verbindung mit einem anderen Modell her, so dass jede Instanz des Deklarationsmodells zu einer Instanz des anderen Modells "gehört".

Zum Beispiel, wenn eine Anwendung Benutzer und Profile enthält und jedes Profil genau einem Benutzer zugewiesen werden kann

```go
type User struct {
  gorm.Model
  Name string
}

// `Profile` gehört zu `User`, `UserID` ist der Fremdschlüssel
type Profile struct {
  gorm.Model
  UserID int
  User   User
  Name   string
}
```

## Fremdschlüssel

Um eine zu einer Beziehung gehörende Eigenschaft zu definieren, muss der Fremdschlüssel vorhanden sein. Der Standard-Fremdschlüssel verwendet den Typnamen des Eigentümers und seinen Primärschlüssel.

Um beispielsweise ein Modell zu definieren, das zu ` User ` gehört, sollte der Fremdschlüssel ` UserID ` lauten.

GORM bietet auch eine Möglichkeit zur Anpassen des Fremdschlüssels:

```go
type User struct {
    gorm.Model
    Name string
}

type Profile struct {
    gorm.Model
  Name      string
  User      User `gorm:"foreignkey:UserRefer"` // benutze UserRefer als Fremdschlüssel
  UserRefer string
}
```

## Assoziations-Fremdschlüssel

Bei einer `belongs to` Beziehung verwendet GORM normalerweise den Primärschlüssel des Eigentümers als Wert des Fremdschlüssels, im obigen Beispiel `User`'s `ID`.

Wenn man einem Benutzer ein Profil zuweist, speichert GORM die `ID` des Benutzers im Feld `UserID` des Profils.

Man kann es mit dem Tag `association_foreignkey` ändern, e.g:

```go
type User struct {
    gorm.Model
  Refer int
    Name string
}

type Profile struct {
    gorm.Model
  Name      string
  User      User `gorm:"association_foreignkey:Refer"` // benutze Refer als Fremdschlüssel der Assoziation
  UserRefer string
}
```

## Arbeiten mit Belongs To

Man kann `belongs to` Assoziationen mit `Related` finden

```go
db.Model(&user).Related(&profile)
//// SELECT * FROM profiles WHERE user_id = 111; // 111 ist die ID des Nutzers
```

Für erweiterte Verwendung verweisen wir auf den [Assoziationsmodus](/docs/associations.html#Association-Mode)