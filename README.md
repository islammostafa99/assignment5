# assignment5
package eg.edu.alexu.datastucture.stack.cs13;

public interface IStack {
    public Object pop();
    public Object peek();
    public void push(Object element);
    public boolean isEmpty();
    public int size();
}

package eg.edu.alexu.datastucture.stack.cs13;
public class Stack implements IStack{
    private LinkedList l = new LinkedList();
    private int top=-1;
    public void push(Object s){
        top++;
        l.add(top,s);
    }
    public Object pop(){
        Object v;
        if(!l.isEmpty()) {
            v = l.get(top);
            l.remove(top);
            top--;
        }else{
            v=null;
        }
        return v;
    }
    public Object peek(){
        Object v;
        if(!l.isEmpty()) {
            v = l.get(top);
        }else{
            v=null;
        }
        return v;
    }
    public boolean isEmpty(){
        if(l.isEmpty()){
            return true;
        }else{
            return false;
        }
    }
    public int size(){
        return top+1;
    }
}


package eg.edu.alexu.datastucture.stack.cs13;
public interface IExpressionEvaluator {
    public String infixToPostfix(String expression);
    public int evaluate(String expression);
}


package eg.edu.alexu.datastucture.stack.cs13;
import java.lang.String;
import java.lang.Object;
public class ExpressionEvaluator implements IExpressionEvaluator{
    private LinkedList m=new LinkedList();
    private Stack b=new Stack();
    public String infixToPostfix(String expression){
        int i,count=0;
        char c,x;
        for(i=0;i<expression.length();i++){
            c=expression.charAt(i);
            int j;
            if(c=='+'||c=='*'||c=='-'||c=='/'||c=='('||c==')'){
                if(b.isEmpty()){
                    b.push(c);
                }else{
                    if(b.peek()==null||b.peek()!=null) {
                        if ((c == '(') || ((char) b.peek() == '(') || (((c == '*') || (c == '/')) && (((char) b.peek() == '+') || ((char) b.peek() == '-')))) {
                            b.push(c);
                        } else if (c == ')') {
                            while ((char) b.peek() != '(') {
                                x = (char) b.pop();
                                m.add(count, x);
                                count++;
                            }
                            b.pop();
                        } else {
                            x = (char) b.pop();
                            m.add(count, x);
                            count++;
                            if (b.peek() == null || b.peek() != null) {
                                if(b.isEmpty()) {
                                    b.push(c);
                                }else{
                                    if ((c == '(') || ((char) b.peek() == '(') || (((c == '*') || (c == '/')) && ((char) b.peek() == '+') || (char) b.peek() == '-')) {
                                        b.push(c);
                                    } else if (c == ')') {
                                        while ((char) b.peek() != '(') {
                                            x = (char) b.pop();
                                            m.add(count, x);
                                            count++;
                                        }
                                        b.pop();
                                    } else {
                                        x = (char) b.pop();
                                        m.add(count, x);
                                        count++;
                                        if (b.peek() == null || b.peek() != null) {
                                            if(b.isEmpty()) {
                                                b.push(c);
                                            }else{
                                                if ((c == '(') || ((char) b.peek() == '(') || (((c == '*') || (c == '/')) && ((char) b.peek() == '+') || (char) b.peek() == '-')) {
                                                    b.push(c);
                                                } else if (c == ')') {
                                                    while ((char) b.peek() != '(') {
                                                        x = (char) b.pop();
                                                        m.add(count, x);
                                                        count++;
                                                    }
                                                    b.pop();
                                                } else {
                                                    x = (char) b.pop();
                                                    m.add(count, x);
                                                    count++;
                                                }
                                            }
                                        }
                                    }
                                    }
                                }
                            }
                        }
                    }
                }else {
                StringBuilder f = new StringBuilder();
                j = i;
                while(c != '+' && c != '*' && c != '-' && c != '/' && c != '(' && c != ')') {
                    f.append(c);
                    j++;
                    if(j<expression.length()) {
                        c = expression.charAt(j);
                    }else {
                        break;
                    }
                }
                f.toString();
                m.add(count,f);
                count++;
                i=j-1;
            }
        }
        while(!b.isEmpty()){
            x=(char)b.pop();
            m.add(count,x);
            count++;
        }
        int size =m.size();
        StringBuilder q=new StringBuilder();
        i=0;
        while(i<size){
            q.append(m.get(i));
            q.append(' ');
            i++;
        }
        String w=q.toString();
        return w;
    }
    public int evaluate(String expression){
        String h =infixToPostfix(expression);
        char c;
        String k,o;
        int i=0,j,num1,num2,res,ev;
        while(i<h.length()) {
            c = h.charAt(i);
            if (c != ' ') {
                if (c == '+' || c == '-' || c == '*' || c == '/') {
                    StringBuilder v=new StringBuilder();
                    StringBuilder e=new StringBuilder();
                    v = (StringBuilder) b.pop();
                    k=v.toString();
                    num2=Integer.parseInt(k);
                    e = (StringBuilder) b.pop();
                    o=e.toString();
                    num1=Integer.parseInt(o);
                    if (h.charAt(i) == '+') {
                        res = num1 + num2;
                        StringBuilder p=new StringBuilder();
                        p.append(res);
                        b.push(p);
                    } else if (h.charAt(i) == '-') {
                        res = num1 - num2;
                        StringBuilder p=new StringBuilder();
                        p.append(res);
                        b.push(p);
                    } else if (h.charAt(i) == '*') {
                        res = num1 * num2;
                        StringBuilder p=new StringBuilder();
                        p.append(res);
                        b.push(p);
                    } else if (h.charAt(i) == '/') {
                        res = num1 / num2;
                        StringBuilder p=new StringBuilder();
                        p.append(res);
                        b.push(p);
                    }
                    i++;
                }else{
                    StringBuilder f=new StringBuilder();
                    j=i;
                    while (c != '+' && c != '-' && c != '*' && c != '/'&&c!=' ') {
                        f.append(c);
                        j++;
                        if (j < h.length()) {
                            c = h.charAt(j);
                        } else {
                            break;
                        }
                    }
                    f.toString();
                    b.push(f);
                    i=j;
                }
            }else {
                i++;
            }
        }
        StringBuilder ss=new StringBuilder();
        ss=(StringBuilder) b.pop();
        String qq=ss.toString();
        ev=Integer.parseInt(qq);
        return ev;
    }
}


package eg.edu.alexu.datastucture.stack.cs13;
import java.util.Scanner;
public class Main {

    public static void main(String[] args) {
        Scanner input=new Scanner(System.in);
        ExpressionEvaluator g=new ExpressionEvaluator();
        String r=new String();
        System.out.println("please Enter the infix:");
        r=input.next();
        System.out.println("the postfix is:"+g.infixToPostfix(r));
        System.out.println("the solution is:"+g.evaluate(r));
    }
}
