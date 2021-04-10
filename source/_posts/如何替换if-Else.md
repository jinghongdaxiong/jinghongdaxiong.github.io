## 免责声明：本文并不肯定或者否定哪一种写法，仅仅为大家提供一些其他的编码思路或者一些值得借鉴的点子

    
    if（condition）{
        do stuff
    } else (otherCondition) {
        do something
    } else {
    
    }

if-Else通常是个糟糕的选择，它导致设计复杂，代码可读性差，重构困难等等。说了这么多如何优化？或者说如何替换if-Else呢？

## 五种方式

### 完全不必要的Else块

    public void PerformOperation(int input) {
        if(intput > 5) {
            // do something
        } else {
            // do something
        } 
    }

只需要删除else块即可简化此过程，如下所示

    public void PerformOperation(int input) {
        if(intput > 5) {
            // do something
            return;
        } 

        // do something
    }

仔细体会一下，那种写法更容易理解？其实就一句话：在满足特定条件的情况下执行某些操作并立即返回，适用于异常流先返回，往下执行的都是正常流业务。

### 价值分配
如果你想要根据某些输入为变量分配新值，如下：

    public static string determineGender(int input) {
        String gender = String.Empty;
        if(input == 0) {
            gender = "male";
        } else if(input == 1) {
            gender = "woman";
        } else if {
            gender = "unknown";
        }
        
        return gender;
    }

上述if-Else很容易被开关取代。如下：


    public static string determineGender(int input) {
        if(input == 0) {
             return "male";
        } else if(input == 1) {
             return "woman";
        } else if {
             return "unknown";
        }
    }

进一步优化，可通过删除else来进一步简化代码

    public static string determineGender(int input) {
        if(input == 0) {return "male"; } 
        if(input == 1) {return "woman"; }  

        return "unknown";
    }
    
若不使用else，则我们将剩下干净的可读代码。这么做的好处是可以迅速得到想要的值。试想：如果已经找到正确的值，继续测试下一个值一点意义也没有。

### 前提条件检查(用三元运算代替if)

通常，我发现，如果方法提供了无效的值，则继续执行是没有意义的。假设有一个方法defineGender方法，要求输入值必须始终为0或者1。
    
    // Input must be 0 or 1
    public String defineGender (int input) {
        // Continue executing logic
    }
在没有参数验证的情况下,执行该方法没有任何意义，或者说太容易出bug了，因此在实际业务之前，我们需要检查一些先决条件。

    // Input must be 0 or 1
    public String defineGender (int input) {
        if(intput < 0) throw new ArgumentException();
        if(intput > 0) throw new ArgumentException();
    
        return input == 0 ? "woman" : "man";
    }
因有明确的输入限制，if可以用三元代替，因此不再需要在结尾处写默认返回值。

### 将 If-Else转换为字典，完全避免 IF-ELSE

假设你需要执行一些操作，这些操作将根据某些条件进行选择，我们知道以后必须添加更多条件操作。

    public void performOp(String operationName) {
        if(operationName == "Op1")) {
           // something 
        } else if (operationName == "Op2") {
           // something 
        } else {
           // default path 
        }
    }

有些人倾向于使用久经考验的 If-Else。如果添加新操作，则只需要简单地添加其他内容即可。很简单!但是，就维护而言,这种方法不是一个号的设计。

知道我们以后需要添加新的操作后，我们可以将If-Else重构为字典。

    public void performOp(String operationName) {
        var operations = new Dictionary<String, Action>();
        operations["Op1"] = () => {// something};
        operations["Op2"] = () => {// something};

        operations[operationName].Invoke();
    }

可读性已经大大提高，并且可以轻松地推断出该代码。注意，仅用于说明目的将字典放置在方法内部。你可以在其他地方定义它。

### 扩展应用程序，完全避免使用If-Else

    public String printOrder(Order order, String formatType) {
        String result = String.Empty;

        if(formatType == "Json") {
            result = JsonSerializer.Serialize(order);
        } else if (formatType == "PlainText") {
            result = $"Id:{order.Id}\nSum: {order.Sum}";
        } else {
            result = "Unknown format";
        }

        return result;
    }

上述代码我们若有新业务增加，则可通过添加if-else来解决，但它违反了开闭原则，且可读性差。    

正确的做法为：遵循SOLID原则，我们通过实施动态类型发现过程（本例中为策略模式）来做到这一点。

**重构的过程如下**

* 使用公共接口将每个分支提取到单独的策略类中。
* 动态查找实现通用接口的所有类。
* 根据输入决定执行哪种策略。

重点是类型发现的工作原理。

    private String printOrder(Order order, String formatType) {

    // Dynamic type discovery process that builds a dictionary
    Dictionary<String, Type> formatterTypes = Assembly
        .GetExecutingAssembly()
        .GetExportedTypes()
        .Where(type => type.getInterfaces().Contains(typeof(IOrderOutputStrategy)))
        .ToDictionary(type => type.GetCustomAttribute<OutputFormatterName>().DisplayName);
    
    Type choseFormatter = formatterTypes[formatType];

    // Try instantiate the formatter -- could have utilized a DI framework here instead
    IOrderOutputStrategy strategy = Activator.CreateInstance(chosenFormatter) as IOrderOutputStrategy;
    if ( strategy is null) throw new InvalidOperationException("No valid formatter selected");
    
    // Execute strategy method
    string result = strategy.ConvertOrderToString(order);
    return result;
    }


















