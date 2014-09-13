# Χρονοπρογραμματισμός Διαδικασιών

Σκοπός της εργασίας είναι η συγγραφή προγραμμάτων σε γλώσσα Java για την επίλυση προβλημάτων χρονοπρογραμματισμού διαδικασιών.

Έστω ότι υπάρχουν n διαφορετικές διαδικασίες: 
```
x[0], x[1], ..., x[n-1]
```

Οι διαδικασίες αυτές υπόκεινται σε `m` περιορισμούς όσον αφορά το χρόνο έναρξής τους. Οι περιορισμοί είναι της μορφής:
```
x[i] >= x[j] + t[k]
```
όπου `0 <= k <= m-1`. Ο παραπάνω περιορισμός σημαίνει ότι η διαδικασία `x[i]` μπορεί να ξεκινήσει `t[k]` χρονικές περιόδους (π.χ., λεπτά) μετά τη διαδικασία `x[j]` αν `t[k] > 0` ή ότι η διαδικασία `x[i]` μπορεί να ξεκινήσει `t[k]` χρονικές περιόδους πριν τη διαδικασία `x[j]` αν `t[k] < 0`.

Αν θεωρήσουμε ότι ξεκινάμε τη χρονική στιγμή `0`, θέλουμε να βρούμε τις ελάχιστες χρονικές στιγμές έναρξης των διαδικασιών που μας δίνονται. 

Για να λύσουμε το πρόβλημα αυτό, κατασκευάζουμε ένα γράφο ο οποίος περιέχει κόμβους που αντιστοιχούν στις διαδικασίες που θέλουμε να προγραμματίσουμε, συν έναν επιπλέον κόμβο:

```
v, x[0], x[1], ..., x[n-1]
```

Οι σύνδεσμοι στο γράφο είναι οι:
```
(v, x[0], 0) 
(v, x[0], 0) 
...
(v, x[n-1], 0) 
```
δηλαδή σύνδεσμοι από τον κόμβο `v` σε κάθε κόμβο του γράφου με βάρος `0` και οι: 
```
(x[i], x[j], -t[k])
```
δηλαδή ένας σύνδεσμος για κάθε περιορισμό με βάρος τo αντίθετο της τιμής της χρονικής περίοδου που υπεισέρχεται στον περιορισμό. 

Στον γράφο που κατασκευάστηκε τρέχουμε τον αλγόριθμο Bellman-Ford (δεδομένου ότι μπορεί να έχει αρνητικά βάρη) για να βρούμε τα συντομότερα μονομάτια από τον κόμβο `v` στους υπόλοιπους κόμβους του γράφου. Αν τα συντόμερα μονοπάτια που θα βρεθούν έχουν μήκη:
```
(0, y[0], y[1], ..., y[n-1])
```
τότε οι αριθμοί:
```
y[0], y[1], ..., y[n-1]
```
είναι μια λύση στο πρόβλημά μας, ώστε η διαδικασία `0` μπορεί να ξεκινήσει τη χρονική στιγμή `y[0]`, η διαδικασία `1` μπορεί να ξεκινήσει τη χρονική στιγμή `y[1]`, κ.ο.κ. Επίσης οι αριθμοί:
```
d + y[0], d + y[1], ...,  d + y[n-1]
```
για κάθε `d` αποτελούν επίσης λύση του προβλήματος. Συνεπώς, αν στα:
```
y[0], y[1], ..., y[n-1]
```
υπάρχουν αρνητικές τιμές, αρκεί να προσθέσουμε στη λύση τον αριθμό `d` ίσο με τη μικρότερη αρνητική τιμή για να πάρουμε μια λύση με θετικές τιμές.

## Παράδειγμα (Εισαγωγή στους 24.4)

Έστω ότι δίνονται οι εξής περιορισμοί:
```
x1 x2 2
x2 x3 -7
x3 x4 3
x1 x4 4
x3 x5 -6
x2 x5 -5
x5 x1 -3
x4 x5 -1
```

Με βάση αυτούς τους περιορισμούς κατασκευάζουμε τον παρακάτω γράφο:

![Παράδειγμα γράφου](example_1_graph.pdf "Παράδειγμα γράφου")
