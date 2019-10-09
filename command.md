# credits

all credits given to Mr. Zheng, from whose blog the major part of the following
content is derived.

# definition

Encapsulate a request as an object, thereby letting you parameterize clients with different requests, queue or log requests, and support undoable operations.

# objects
- Command
- ConcreteCommand
- Invoker
- Receiver
- Client

# examples

『服務生 (Invoker)』 儲存好『命令 (Command)』並呼叫，
『廚師 (Receiver)』 收到訊息，開始料理。

# code

```
import java.util.LinkedList;
import java.util.Queue;

public class 餐廳 {

    public static void main(String[] args) {

        廚師 cook = new 廚師(); // 準備廚師 cook (Receiver)

        服務生 waiter = new 服務生(); // 準備 服務生 waiter (Invoker)

        // 準備命令 並 設置廚師
        命令 command = new 點牛排命令(cook);
        命令 command2 = new 點豬排命令(cook);

        // 將準備好的命令 告訴服務生
        waiter.addOrder(command);
        waiter.addOrder(command2);

        // 讓服務生送出命令
        // 開始準備餐點
        waiter.sendOrders();

    }
}

interface 命令 {
    void execute();
}

class 點牛排命令 implements 命令 {

    private 廚師 cook;

    public 點牛排命令(廚師 cook) {
        this.cook = cook;
    }

    @Override
    public void execute() {
        cook.cookSteak();
    }
}

class 點豬排命令 implements 命令 {

    private 廚師 cook;

    public 點豬排命令(廚師 cook) {
        this.cook = cook;
    }

    @Override
    public void execute() {
        cook.cookPork();
    }
}


class 服務生 {
    private Queue<命令> orders = new LinkedList<>(); // 巨集佇列命令

    public void addOrder(命令 command) {
        orders.offer(command);
    }

    public void cancelOrder(命令 command) {
        orders.remove(command);
    }

    public void sendOrders() {

        while (!orders.isEmpty()) {
            命令 cmd = orders.poll();
            cmd.execute();
        }
    }

}

class 廚師 {

    void cookSteak() {
        System.out.println("牛排來嚕~");
    }

    void cookPork() {
        System.out.println("豬排來嚕~");
    }
}

// Result:
// 牛排來嚕~
// 豬排來嚕~
```

# remarks

這樣的設計，符合了 單一職責原則，
客戶端只需對服務生下命令，不需知道餐點是如何實現，
服務生 (Invoker) 只負責儲存訂單 (命令) 並 點餐，廚師 (Receiver) 可專注於料理。

這也是 命令模式 (Command Pattern) 最大的好處:
將『引發命令的物件』與『實際執行操作的物件』隔離開來。

系統邏輯 的隔離，大大的增加了擴充性:
新增一個命令物件，並配置 欲呼叫的 Receiver 與其操作，即可增加新功能。

並確保了程式碼的 覆用 (reuse)，
無形中避免掉 『程式碼的壞味道 (Bad smells in Code)』中，
最悲劇的 『重複的程式碼 (Duplicated Code)』。