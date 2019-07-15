- 简单工厂模式  
用静态方法
```
class User {
    constructor(opt) {
        this.name = opt.name;
    }

    static function getInstance(role) {
        switch(role) {
            case 'superAdmin': 
                return new User({name: "super admin"});
                break;
            case 'admin':
                return new User({name: 'admin'});
                break;
            case 'user':
                return new User({name: 'user'});
                break;
            default:
                throw new Error('参数错误, 可选参数:superAdmin、admin、user');
        }
    }
}

let superAdmin = User.getInstance('superAdmin');
```

- 工厂方法模式    
用new.target模拟抽象类
```
class User {
    constructor(name = '') {
        if (new.target === User) {
            throw new Error('抽象类不能实例化!');
        }

        this.name = name;
    }
}

class UserFactory extends User {
    constructor(name) {
        super(name);
    }

    create(role) {
        switch(role) {
            case 'superAdmin':
                return new UserFactory({name: 'super admin'});
                break;
            case 'admin':
                return new UserFactory({name: 'admin'});
                break;
            case 'user':
                return new UserFactory({name: 'user'});
                break;
            default: 
                throw new Error('参数错误, 可选参数:superAdmin、admin、user');
        }
    }
}

let UserFactory = new UserFactory();
let superAdmin = UserFactory.create('superAdmin');
```

- 抽象工厂模式  
类簇
```
function getAbstractUserFactory(role) {
    switch(role) {
            case 'superAdmin':
                return UserofSuperAdmin;
                break;
            case 'admin':
                return UserofAdmin;
                break;
            case 'user':
                return UserofUser;
                break;
            default: 
                throw new Error('参数错误, 可选参数:superAdmin、admin、user');
        }
}

let superAdmin = getAbstractUserFactory('superAdmin');
```