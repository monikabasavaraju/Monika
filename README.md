import java.util.*;

public class Solver
{
    private ArrayList<Item> menu;
    private ArrayList<String[]> solutions;

    public static class Item
    {
        public String name;
        public int price;

        public Item(String name, int price)
        {
            this.name = name;
            this.price = price;
        }
    }

    public Solver(ArrayList<Item> menu)
    {
        this.menu = menu;
    }

    public ArrayList<String[]> solve(int budget)
    {
        solutions = new ArrayList<String[]>();
        solve(new ArrayList<Item>(), 0, budget);
        return solutions;
    }

    private void solve(ArrayList<Item> items, int first, int budget)
    {
        if(budget==0)
        {
            // We have found a solution, store it
            solutions.add(items.stream().map(e -> e.name).toArray(String[]::new));
        }
        else
        {
            // Search for an item that fits in the budget
            for(int i=first;i<menu.size();i++)
            {
                Item item = menu.get(i);
                if(item.price<=budget)
                {
                    items.add(item);
                    solve(items, i, budget-item.price);
                    items.remove(items.size()-1);
                }
            }
        }
    }

    public static void main(String[] args)
    {
        ArrayList<Item> menu = new ArrayList<Item>();
        menu.add(new Item("TOI", 55));
        menu.add(new Item("HINDU", 45));
        menu.add(new Item("ET", 75));
        menu.add(new Item("BM", 85));
        menu.add(new Item("HT", 90));

        Solver solver = new Solver(menu);
        ArrayList<String[]> solutions = solver.solve(550);
        for(int i=0;i<solutions.size();i++)
            System.out.println("Solution "+(i+1)+": "+Arrays.toString(solutions.get(i)));
    }
}
