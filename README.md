import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.io.FileWriter;
import java.io.IOException;
import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.Random;

public class UserInterface extends JFrame {
    private JTextArea textArea;

    public UserInterface() {
        super("Menu Example");
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setSize(400, 300);
        setLocationRelativeTo(null);

        JMenuBar menuBar = new JMenuBar();
        setJMenuBar(menuBar);

        JMenu optionsMenu = new JMenu("Options");
        menuBar.add(optionsMenu);

        createMenuItem(optionsMenu, "Display Date and Time", this::displayDateTime);
        createMenuItem(optionsMenu, "Write to log.txt", this::writeToLog);
        createMenuItem(optionsMenu, "Change Background Color", this::changeBackgroundColor);
        createMenuItem(optionsMenu, "Exit", this::exitProgram);

        textArea = new JTextArea(10, 40);
        JScrollPane scrollPane = new JScrollPane(textArea);
        add(scrollPane);

        setVisible(true);
    }

    private void createMenuItem(JMenu menu, String label, ActionListener actionListener) {
        JMenuItem menuItem = new JMenuItem(label);
        menuItem.addActionListener(actionListener);
        menu.add(menuItem);
    }

    private void displayDateTime(ActionEvent e) {
        SimpleDateFormat dateFormat = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        textArea.setText(dateFormat.format(new Date()));
    }

    private void writeToLog(ActionEvent e) {
        String content = textArea.getText();
        try (FileWriter writer = new FileWriter("log.txt")) {
            writer.write(content);
            showInfoMessage("Content written to log.txt", "Success");
        } catch (IOException ex) {
            showErrorMessage("Error writing to log.txt: " + ex.getMessage(), "Error");
        }
    }

    private void changeBackgroundColor(ActionEvent e) {
        float randomHue = new Random().nextFloat();
        Color color = Color.getHSBColor(randomHue, 0.8f, 0.8f);
        textArea.setBackground(color);
    }

    private void exitProgram(ActionEvent e) {
        System.exit(0);
    }

    private void showInfoMessage(String message, String title) {
        JOptionPane.showMessageDialog(this, message, title, JOptionPane.INFORMATION_MESSAGE);
    }

    private void showErrorMessage(String message, String title) {
        JOptionPane.showMessageDialog(this, message, title, JOptionPane.ERROR_MESSAGE);
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(UserInterface::new);
    }
}

