# 🧩 Entity–Relationship Diagram Documentation

This document describes the **Entity–Relationship (ER) model** for our Pokémon world dataset.  
The model captures the main entities, attributes, and relationships that define the interactions between Pokémon, Trainers, Gyms, and Battles.

---

## 📊 ER Diagram

Below is the conceptual ER diagram for the project:

📄 [View the ER Diagram (PDF)](./pokemon_er_diagram.pdf)

---

## 🧱 Entities and Attributes

### 🟩 **Pokémon**
Represents each Pokémon species, including all its battle attributes.

**Attributes**
- **ID** *(Primary Key)*
- **Number** – National Pokédex number
- **Name**
- **HP**, **Attack**, **Defense**, **Special Attack**, **Special Defense**, **Speed**
- **Total** – *Derived attribute* (sum of all six stats, shown with a dashed ellipse)

**Relationships**
- `has_type` → connects Pokémon to one or two **Types**
- `has_form` → connects Pokémon to its **Form** (e.g., Alolan, Mega)
- `evolves_to` → recursive relationship describing evolution chains
- `owned_by` (via `owns`) → the **Trainer** that possesses the Pokémon
- `fights_in` → connects Pokémon to the **Battle** it participates in

---

### 🟩 **Form**
Describes alternative versions of a Pokémon species (e.g., “Alolan”, “Galarian”, “Mega”).

**Attributes**
- **ID** *(Primary Key)*
- **Name**

**Relationship**
- `has_form` with Pokémon (one Pokémon can have zero or many forms)

---

### 🟩 **Type**
Represents Pokémon elemental types such as Fire, Water, or Grass.

**Attributes**
- **ID** *(Primary Key)*
- **Name**

**Relationships**
- `has_type` with Pokémon (each Pokémon can have 1–2 types)
- `specializes_in` with **Gym** (each Gym specializes in one Type)

---

### 🟩 **Trainer**
Represents a Pokémon trainer, either player or non-player character.

**Attributes**
- **ID** *(Primary Key)*
- **Name**

**Relationships**
- `owns` → Pokémon they possess
- `leads` → Gym they manage (if any)
- `participates_in` → Battles they take part in  
  - includes the attribute **hasWon** on the relationship to mark the winner

---

### 🟩 **Gym**
Represents a Pokémon Gym led by a Trainer.

**Attributes**
- **ID** *(Primary Key)*
- **Name**
- **Location**

**Relationships**
- `leads` ← **Trainer** (one Trainer leads one Gym)
- `specializes_in` → **Type**
- `hosts` → **Battle** (a Gym can host multiple Battles)

---

### 🟩 **Battle**
Represents a Pokémon battle event.

**Attributes**
- **ID** *(Primary Key)*
- **Date** – when the battle took place

**Relationships**
- `participates_in` → **Trainer** (two Trainers per battle; attribute **hasWon** shows outcome)
- `fights_in` → **Pokémon** (Pokémon involved in the battle)
- `hosts` ← **Gym** (optional, if the battle occurs in a Gym)

---

## 🧠 Additional Notes

- **Derived Attribute:** `Total` in Pokémon is not stored but computed as the sum of all base stats.  
- **Cardinalities:**
  - Pokémon–Type: *(1..2)*
  - Pokémon–Form: *(0..1)*
  - Trainer–Pokémon: *(1..*)*
  - Trainer–Battle: *(2..2)*
  - Gym–Trainer: *(1..1)*
  - Gym–Battle: *(0..*)*
- **Design Principle:**  
  This schema models realistic interactions while remaining general enough for both relational (SQL) and graph (Neo4j) databases.

---

## 🏁 Summary

This ER model provides a structured representation of the Pokémon universe:
- **Pokémon** link game attributes, forms, and evolutions.  
- **Trainers** interact with Pokémon and Gyms.  
- **Gyms** specialize in certain Pokémon types and can host **Battles**.  
- **Battles** connect all major entities and store outcome data.  

The model balances completeness and simplicity, ensuring it’s suitable for implementation, querying, and extension in future phases of the project.

---

📁 *File location:* `documentation/er-diagram/er_diagram_explanation.md`
