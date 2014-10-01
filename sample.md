/*
 * JRConnection.java
 *
 * Created on March 11 2012, 16:16
 *
 */

package JRDataBase;

import SDR_Bridge.MainFrame;
import javax.swing.*;
import java.sql.*;
import org.jdesktop.swingx.JXTable;

/**
 *
 * @author Syed
 */
public class JRConnection {
    public Connection jrcon;
    Statement stat;
    public ResultSet res;
    MainFrame mainframe;
    public JRConnection(MainFrame mainframe){
    this.mainframe =mainframe;
 


    
    
    }    
    
    /** This method establishes the connection pool with database,its main role is to provide database connection independently from the database nature (we can use oracle, My sql, sql server and so on)
     */
     
 public boolean connect(String url,String driver,String user,String password) {
     mainframe.setCursor(new java.awt.Cursor(java.awt.Cursor.WAIT_CURSOR));
     try{
     Class.forName(url);
      jrcon = DriverManager.getConnection(driver, user, password);
      stat = jrcon.createStatement();
      mainframe.setCursor(new java.awt.Cursor(java.awt.Cursor.DEFAULT_CURSOR));
     return true;   
     }
     catch(Exception e) {
     System.out.println(e.getMessage());
     mainframe.setCursor(new java.awt.Cursor(java.awt.Cursor.DEFAULT_CURSOR));
     return false;            
     }
    
     }
//**********************************************************************************************************
/** this method extract the field from the database table and store it instantly in front end components.
 */
 //**********************************************************************************************************

 
public String JRSelect(Connection con, String req){
  try{   
mainframe.setCursor(new java.awt.Cursor(java.awt.Cursor.WAIT_CURSOR));

  stat = con.createStatement();
  res = stat.executeQuery(req);
  res.next();
  mainframe.setCursor(new java.awt.Cursor(java.awt.Cursor.DEFAULT_CURSOR));
  return res.getString(1);
  }
  catch(Exception e){
    try{
        mainframe.setCursor(new java.awt.Cursor(java.awt.Cursor.WAIT_CURSOR));
  mainframe.isDBConnected = mainframe.con.connect(mainframe.DBDriver,mainframe.DBUrl, mainframe.DBUser, mainframe.DBPassword);
  this.jrcon = mainframe.con.jrcon;


  stat = jrcon.createStatement();
  res = stat.executeQuery(req);
  res.next();
  mainframe.setCursor(new java.awt.Cursor(java.awt.Cursor.DEFAULT_CURSOR));
  return res.getString(1);
    }catch(Exception exp){
    System.out.print(exp.getMessage());
    mainframe.setCursor(new java.awt.Cursor(java.awt.Cursor.DEFAULT_CURSOR));
  return "undefined";
    }
  }
 }


//**********************************************************************************************************
/** This method is used to extract table result and store it in JTable component.
 */
 //**********************************************************************************************************

 
public JXTable JRSelectResultSet(Connection con, String req,String[] columns){
  try{
      mainframe.setCursor(new java.awt.Cursor(java.awt.Cursor.WAIT_CURSOR));
  stat = con.createStatement();
  res = stat.executeQuery(req);
  mainframe.setCursor(new java.awt.Cursor(java.awt.Cursor.DEFAULT_CURSOR));
  return new dbswing.dbTable(res,columns);
  }
  
  catch(Exception e){
      try{
          mainframe.setCursor(new java.awt.Cursor(java.awt.Cursor.WAIT_CURSOR));
        mainframe.isDBConnected = mainframe.con.connect(mainframe.DBDriver,mainframe.DBUrl, mainframe.DBUser, mainframe.DBPassword);
        this.jrcon = mainframe.con.jrcon;

          stat = jrcon.createStatement();
          res = stat.executeQuery(req);
mainframe.setCursor(new java.awt.Cursor(java.awt.Cursor.DEFAULT_CURSOR));
  return new dbswing.dbTable(res,columns);
      }catch(Exception exp){
         JOptionPane.showMessageDialog(null,"ERREUR : Déconnexion !!","SDR System Control",JOptionPane.ERROR_MESSAGE);
         mainframe.setCursor(new java.awt.Cursor(java.awt.Cursor.DEFAULT_CURSOR));
  return null;
      }
  }

 } 
//**********************************************************************************************************
/** This method is used to update field in database instantly
 */
 //**********************************************************************************************************
public void JRUpdate(Connection con, String req,MainFrame mainframe){

try{
mainframe.setCursor(new java.awt.Cursor(java.awt.Cursor.WAIT_CURSOR));
stat = con.createStatement();    
stat.executeUpdate(req);
mainframe.isExecuted = true;
mainframe.setCursor(new java.awt.Cursor(java.awt.Cursor.DEFAULT_CURSOR));
}
catch(Exception e){
    try{
        mainframe.setCursor(new java.awt.Cursor(java.awt.Cursor.WAIT_CURSOR));
        Class.forName(mainframe.DBUrl);
        mainframe.con.jrcon = DriverManager.getConnection(mainframe.DBDriver, mainframe.DBUser, mainframe.DBPassword);
        stat = mainframe.con.jrcon.createStatement();
        stat.executeUpdate(req);

mainframe.isExecuted = true;
mainframe.setCursor(new java.awt.Cursor(java.awt.Cursor.DEFAULT_CURSOR));
    }catch(Exception exp){
 JOptionPane.showMessageDialog(null,"ERREUR de Connexion:","SDR System Control",JOptionPane.ERROR_MESSAGE);
 mainframe.setCursor(new java.awt.Cursor(java.awt.Cursor.DEFAULT_CURSOR));
   }
}
}



 
}


