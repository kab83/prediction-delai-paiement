
# 📊 Prédiction du délai de paiement à partir de fichiers FEC

## 🧠 Objectif du projet
Utiliser des fichiers FEC consolidés pour entraîner un modèle de machine learning capable de prédire le **délai de paiement** entre la date d'écriture comptable et la date de paiement.

---

## ⚙️ Données simulées
Des fichiers FEC fictifs ont été générés avec :
- `EcritureDate` (date comptable)
- `DatePaiementClient` (date effective de paiement)
- `CompteNum`, `JournalLib`, `Libelle`, `Montant`, `Debit`, `Credit`, `ClientID`, etc.

Le `délai` a été calculé comme :
```python
df['Delai'] = (df['DatePaiementClient'] - df['EcritureDate']).dt.days
```

---

## 🔬 Entraînement du modèle
Un `RandomForestRegressor` a été utilisé pour la prédiction :

```python
from sklearn.ensemble import RandomForestRegressor
from sklearn.model_selection import train_test_split
from sklearn.metrics import mean_absolute_error, mean_squared_error, r2_score

X = df[['Montant', 'Debit', 'Credit']]  # features
y = df['Delai']  # target

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=42)
model = RandomForestRegressor(n_estimators=100, random_state=42)
model.fit(X_train, y_train)
```

---

## 📈 Résultats du modèle

```text
MAE  : 18.04 jours
RMSE : 21.05 jours
R²   : 0.0098
```

➡️ Le modèle prédit globalement une moyenne stable (~50 jours), sans capter correctement la variabilité du délai.

---

## 📊 Visualisation des prédictions vs réel

```python
plt.scatter(y_test, y_pred)
plt.xlabel("Vrai délai")
plt.ylabel("Délai prédit")
plt.title("Prédiction vs Réel")
plt.grid(True)
plt.show()
```

---

## 🛠️ Améliorations possibles
- Ajouter des **données clients** (ancienneté, type, pays…)
- Ajouter les **informations de cycle** (périodicité, saisonnalité)
- Élargir les **features textuelles** (via NLP ou vectorisation des libellés)

---

## 🚀 Fichier Jupyter prêt à exécution
Inclus ci-dessous : toutes les étapes (de la simulation au modèle final)
