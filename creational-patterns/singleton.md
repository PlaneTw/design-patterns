# Singleton Pattern（獨體模式）

### Defined
保證一個Class只會有一個實體，提供一個全域存取點。

### UML
![Image of UML](https://raw.githubusercontent.com/loredanacirstea/staruml-design-patterns/master/generated/Model/loretek/design_patterns/creational/singleton/singleton.png)

### Story
有一座吊橋，原為單進單出，人數乘載限制為10人。每一次當橋上已經有9人時，這時候有2人要走過吊橋。

```javascript
const limit = 10;

class SuspensionBridge {
    constructor () {
        this.people = 9;
    }

    entry () {
        if (this.people === limit) {
            console.log('乘載人數已滿');
            return;
        }

        this.people += 1;
        this.notice(this.people);
    };

    exit  ()  {
        this.people -= 1;
        this.notice(this.people);
    };

    notice (people) {
       let accessCount = limit - people;
       if (accessCount >= 0) {
           console.log('目前橋上人數:' + people + '，可在通行人數: ' + accessCount);
       }
   };
}

let entrance = new SuspensionBridge();
entrance.entry();
entrance.entry();
```

有一天，B岸居民說用時段輪流入口交換，這樣讓生活上造成方便，因此建議改成兩邊都可以進出。
此時從A岸進入9人時，這時候A岸和B岸都有1人要走過吊橋。
因為系統未更新，讓系統誤認成這是兩座吊橋。
```javascript
const limit = 10;

class SuspensionBridge {
    constructor () {
        this.people = 9;
    }

    entry () {
        if (this.people === limit) {
            console.log('乘載人數已滿');
            return;
        }

        this.people += 1;
        this.notice(this.people);
    };

    exit  ()  {
        this.people -= 1;
        this.notice(this.people);
    };

    notice (people) {
       let accessCount = limit - people;
       if (accessCount >= 0) {
           console.log('目前橋上人數:' + people + '，可在通行人數: ' + accessCount);
       }
   };
}

let entranceA = new SuspensionBridge();
let entranceB = new SuspensionBridge();
entranceA.entry();
entranceB.entry();
```

因此需要把系統更新，讓系統不會判斷成是兩座吊橋。
```javascript
const limit = 10;
let instance = null;

class SuspensionBridge {
    constructor () {
        if (!instance) {
            instance = this;
        }

        this.people = 9;

        return instance;
    }

    entry () {
        if (this.people === limit) {
            console.log('人數已滿');
            return;
        }

        this.people += 1;
        this.notice(this.people);
    };

    exit  ()  {
        this.people -= 1;
        this.notice(this.people);
    };

    notice (people) {
       let accessCount = limit - people;
       if (accessCount >= 0) {
           console.log('目前橋上人數:' + people + '，可在通行人數: ' + accessCount);
       }
   };
}


let entranceA = new SuspensionBridge();
let entranceB = new SuspensionBridge();
entranceA.entry();
entranceB.entry();
```

### When to use
當你需要固定實體個數時。

### Pros
- Better memory management
- Lazy initialization (in a single-thread system)

### Cons
- Unit testing is more difficult
