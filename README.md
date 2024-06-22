# mvc-data-table

Modify this source code with implementation of MVC Design Pattern

import java.awt.BorderLayout;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.ResultSetMetaData;
import java.sql.Statement;
import java.util.ArrayList;
import java.util.Vector;
import javax.swing.JFrame;
import javax.swing.JPanel;
import javax.swing.JScrollPane;
import javax.swing.JTable;

public class Main extends JFrame {
    public Main(String name) {
        super(name);
        ArrayList columnNames = new ArrayList();
        ArrayList data = new ArrayList();

        try {
            Class.forName("com.mysql.cj.jdbc.Driver");
            String url = "jdbc:mysql://localhost:3306/database_sample";

            String userid = "root";
            String password = "admin";

            String sql = "SELECT * FROM Customers";

            Connection connection = DriverManager.getConnection(url, userid, password);

            Statement stmt = connection.createStatement();
            ResultSet rs = stmt.executeQuery(sql);
            ResultSetMetaData md = rs.getMetaData();

            int columns = md.getColumnCount();
            for (int i = 1; i <= columns; i++) {
                columnNames.add(md.getColumnName(i));
            }
            while (rs.next()) {
                ArrayList row = new ArrayList(columns);
                for (int i = 1; i <= columns; i++) {
                    row.add(rs.getObject(i));
                }
                data.add(row);
            }
            Vector columnNamesVector = new Vector();
            Vector dataVector = new Vector();
            for (int i = 0; i < data.size(); i++) {
                ArrayList subArray = (ArrayList) data.get(i);
                Vector subVector = new Vector();
                for (int j = 0; j < subArray.size(); j++) {
                    subVector.add(subArray.get(j));
                }
                dataVector.add(subVector);
            }
            for (int i = 0; i < columnNames.size(); i++)
                columnNamesVector.add(columnNames.get(i));

            JTable table = new JTable(dataVector, columnNamesVector) {
                public Class getColumnClass(int column) {
                    for (int row = 0; row < getRowCount(); row++) {
                        Object o = getValueAt(row, column);
                        if (o != null) {
                            return o.getClass();
                        }
                    }
                    return Object.class;
                }
            };
            JScrollPane scrollPane = new JScrollPane(table);
            getContentPane().add(scrollPane);
            JPanel buttonPanel = new JPanel();
            getContentPane().add(buttonPanel, BorderLayout.SOUTH);
        } catch (Exception e) {
            e.printStackTrace();
            System.out.println("Connection Failed");
        }

    }

    public static void main(String[] args) throws Exception {
        Main frame = new Main("Customer List");

        frame.setDefaultCloseOperation(EXIT_ON_CLOSE);
        frame.pack();
        frame.setLocationRelativeTo(null);
        frame.setVisible(true);
    }
}

## Collaborate with GPT Engineer

This is a [gptengineer.app](https://gptengineer.app)-synced repository 🌟🤖

Changes made via gptengineer.app will be committed to this repo.

If you clone this repo and push changes, you will have them reflected in the GPT Engineer UI.

## Tech stack

This project is built with React and Chakra UI.

- Vite
- React
- Chakra UI

## Setup

```sh
git clone https://github.com/GPT-Engineer-App/mvc-data-table.git
cd mvc-data-table
npm i
```

```sh
npm run dev
```

This will run a dev server with auto reloading and an instant preview.

## Requirements

- Node.js & npm - [install with nvm](https://github.com/nvm-sh/nvm#installing-and-updating)
