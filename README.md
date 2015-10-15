# project-esiag



public class GmapDistance1 {
	 

	 static TestData tst;
	static String outputString  ="";
	 static BufferedReader reader ;
	static JTextField field;                           
	
	static List<String> ss; 
	 static List<String> sst; 
	
   static  List<String> listes = Arrays.asList("Hôpital Intercommunal",  "hôpital Henri Mondor","hôpital Bicêtre","Hôpital Paul-Brousse, hôpital Sainte-Camille") ;;
   
	static final String  destination ="%2040%20Avenue%20de%20Verdun,%2094000,%20Creteil|%2051%20Avenue%20du%20Marechal%20de%20Lattre%20de%20Tassigny,%2094010%20Creteil|%2078%20Rue%20du%20General%20Leclerc%20,%2094270%20Le%20Kremlin%20Bicetre|%2012%20Avenue%20Paul%20Vaillant%20Couturier,%2094800%20Villejuif|%20Rue%20des%20Peres%20Camilliens%20,%2094366%20Bry%20sur%20Marne&sensor=true"; 
	static final String dest ="Juvisy";
	static public String originse ;
	 
	 
	 

     public static String getOrigins() {
		                 return originse;
	                                     }



	 public static void setOrigins(String origins) {
		           GmapDistance1.originse = origins;
	                                     }
	 
	 
	 
	 
	
			      
			      
			      
			      
	
		     
	 
	 
	 
	
public static void main(String[] args) throws IOException {

	  try{
		  			// Frame
			  
		 
			 JFrame frame = new JFrame("Get Adresses");
		 // size of frame
		 frame.setSize(300,300);
		 JPanel pan = new JPanel();
		  field = new JTextField();
		 field.setPreferredSize(new Dimension(100,25));
		
		  
		 
		// container
		 BorderLayout br = new BorderLayout();
		 JPanel panel1 = new JPanel();
		 JPanel panel2 = new JPanel();
		
		 panel1.setLayout(new FlowLayout());
		
		 
		 JLabel label = new JLabel("Addresse");
		
		 
		 
		 pan.setLayout(new BorderLayout());
		 panel1.add(label);
		 panel1.add(field);
		
		 pan.add(panel1,br.NORTH);
		
		 pan.add(panel2,br.SOUTH);
		 
		 JButton bouton = new JButton("Rechercher");
		 panel2.add(bouton, br.SOUTH);
		 frame.getContentPane().add(pan);
		 frame.setVisible(true);
			 
			bouton.addActionListener(new ActionListener(){
				

				@Override
				public void actionPerformed(ActionEvent e) {
					// TODO Auto-generated method stub
					
					 originse = field.getText();
					
					if(originse!= null){
						try{
							
							
							JOptionPane.showMessageDialog(null, "Adress was determined");
							
						
						
							 File file = new File("C:/Users/Dell/Desktop/log.txt");                                 
								
								PrintStream mm = new PrintStream(file);
								System.setOut(mm);
								
								
							// here i call api distancematrix
						URL url = new URL("https://maps.googleapis.com/maps/api/distancematrix/json?origins="+originse+"&destinations="+destination+"");
						// here i use proxy to allow to my class to connect to proxy
						  Proxy proxy = new Proxy(Proxy.Type.HTTP, new InetSocketAddress("proxy.inside.esiag.info", 3128));
						//  I defined my httpUrlConnection to use my proxy 
					      HttpURLConnection conn = (HttpURLConnection) url.openConnection(proxy);
					      
					      
					      
					      // here i defined method get to retrieve data from api matrix
					      conn.setRequestMethod("GET");
					      String line = "";
					       reader = new BufferedReader(
					      new InputStreamReader(conn.getInputStream()));
					       // here i put in  place encodage
					       Charset.forName("UTF-8");
					      while ((line = reader.readLine()) != null) {
					    	  // serialize object
					          outputString += line;
					          
					          
					      }
					      // i m initiate  class TestData
					      tst = new TestData();
					      // here i method of class TestData
							tst.GetGeocoding(originse, destination);
							 
		                
					      // here i defined JSONObject
					   JSONObject objt = new JSONObject(outputString);
					   JSONArray array = objt.getJSONArray("rows"); 
					   JSONArray arrays = objt.getJSONArray("destination_addresses");
					  // System.out.println(list);
					   
					   for(int i =0;i<array.length();i++){
						   
						
						   JSONObject row = array.getJSONObject(i); // Get row object
			                JSONArray elements = row.getJSONArray("elements"); // Get all elements for each row as an array

			                for(int j=0; j < elements.length(); j++) { // Iterate each element in the elements array
			                    JSONObject element =  elements.getJSONObject(j); // Get the element object
			                    JSONObject duration = element.getJSONObject("duration"); 
			                    // Get duration sub object
			                   
			                    String text = duration.getString("text");
			                  
			                  
			                   
			                    	
			                    
			                    
			                    
			                    
			                    
			                    //retrieve distance
			                    JSONObject distance = element.getJSONObject("distance"); 
			                   
			                   
			                    
			                    String text1 = distance.getString("text");
			                    ss = new ArrayList();
				                  ss.add(text);
				                  
				                   String vv = ss.get(0);
				                  
				                  
				              
				                   sst = new ArrayList();
				                  sst.add(text1);
				               
				               
					               
				                  
			                    
			                 
			                   System.out.println( listes);
			                   System.out.println("Temps � faire"+ss);
			                   
				                System.out.println("Distance � parcourir"+sst);
				                
				                System.out.println("\n");
					               
			                 
			              
			                }
				             
			                
					   }   
						  
					   
					   }
			                 
			                   
			                   		
						
						
						
						catch(Exception ex){ex.printStackTrace();}					
					}
					
				}
				
			});
			
	  }
	  catch(Exception e){e.printStackTrace();}	
	  
	   
	  
	  
		  
	 
}
}
