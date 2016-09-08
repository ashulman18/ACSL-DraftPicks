# ACSL-DraftPicks

//will input using spaces
//gonna want to use a sorting algorithm for outputs
//fix zeros for output

import javax.swing.JOptionPane;
import java.util.ArrayList;
import java.util.Arrays;

public class DraftPicks
{
	private ArrayList<String> draft;
	private double[] arr, player18;
   private double []player16;
	private double length, value, guarantee;
   private double inj16, inj18;
	
	public DraftPicks()//initialize the List of player stats, constructor
	{
		int player=1;
		draft=new ArrayList<String>();
		while(player<11)
		{
         draft.add(JOptionPane.showInputDialog("Stats for the top "+player+" first round "+
            "draft pick seperated by spaces: the length of the contract in years, the value "+
            "of \n the contract in millions of dollars, and the guaranteed money in millions of dollars."));
			
         player++;
		}
      fillPlayer();
		//inj16=.03;
		//inj18=.03375;
	}
	
   public double findExpected16(double len, double val, double guar)
	{
		inj16=.03;
      return ((1-(len*inj16))*(val+(len*inj16*guar)));
	}
	
   private double findExpected18(double len, double val, double guar)
	{
		inj18=.03375;
      return ((1-(len*inj18))*(val+(len*inj18*guar)));
	}
   
   private void fillPlayer() //fills player arrays with the expected salaries
	{
		int temp = 0;
		int count=0;
		String subs="";
      String tempString="";
      player16=new double[10];
		player18=new double[10];
      for(int i=0;i<draft.size();i++) //loop through List
		{
         tempString=draft.get(i);
         while(tempString.length()>0&&temp!=-1) //loop through each String in List
			{
            temp = tempString.indexOf(" ");
				if(temp!=-1)
				{
               subs=(tempString).substring(0,temp);
               if(count==0)
                  length=Double.parseDouble(subs);
               else if(count==1)
                  value=Double.parseDouble(subs);
            }
            else if(count==2)
               guarantee= Double.parseDouble(tempString);
         	count++;
            tempString=tempString.substring(temp+1);
         }
         player16[i]=findExpected16(length,value,guarantee);
			player18[i]=findExpected18(length,value,guarantee);
         temp=0;
      }
   }
	public void findInfo() //method to be called for the 5 lines of output
	{
      //at this point i have two full arrays of player expected values
      double[]temp=player16;
      Arrays.sort(player16);//least to greatest
      double num =player16[9];
      int save=0;
      for(int i=0;i<temp.length;i++)
      {
         if(num==temp[i])
            save=i;
      }
      Arrays.sort(player18);
      System.out.println(player16[9]-player16[0]); //range
      System.out.println((player18[9]+player18[0])/2); //midrange
      System.out.println(player16[9]+" #"+save);//-----highest and who
      int sum=0;
      for(int i=0;i<player18.length;i++)
      		sum+=player18[i];
      System.out.println(sum/10); //average
      System.out.println((player16[5]+player16[4])/2); //check which season after
   }
   public static void main(String[]args)
   {
      
      DraftPicks draft=new DraftPicks();
      draft.findInfo();
   }
}
/*5 57.5 30
6 56.5 29
6 72 34
6 60 26
5 51 23
5 50 21
5 49 19
5 33.4 17.177
5 23 15.6
5 18.9 13.8
Sample Output

513750
438333
65160000 by #3
42698237
9900000*/
