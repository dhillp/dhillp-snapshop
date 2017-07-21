/*
 * Pamaldeep Singh Dhillon
 * SnapShopGUI.java
 * 
 * TCSS 305 - Autumn 2015
 * Assignment 4 - SnapShop
 */

package gui;

import filters.EdgeDetectFilter;
import filters.EdgeHighlightFilter;
import filters.FlipHorizontalFilter;
import filters.FlipVerticalFilter;
import filters.GrayscaleFilter;
import filters.SharpenFilter;
import filters.SoftenFilter;

import image.PixelImage;

import java.awt.BorderLayout;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

import java.io.File;
import java.io.IOException;

import javax.swing.ImageIcon;
import javax.swing.JButton;
import javax.swing.JFileChooser;
import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.JOptionPane;
import javax.swing.JPanel;

/**
 * SnapShop GUI which opens and filters images based on user input.
 * 
 * @author Pamaldeep Dhillon
 * @version 03 November 2015
 */
public class SnapShopGUI {
    /** Number of buttons for the south of the GUI.*/
    private static final int NUM_OF_SOUTH_BUTTONS = 3;
    /** Number of buttons for the north of the GUI.*/
    private static final int NUM_OF_NORTH_BUTTONS = 7;
    /** True if an image file is open, false otherwise.*/
    private boolean myImageOpen;
    /** JFileChooser used to save and open files.*/
    private final JFileChooser myChooser;
    /** Array of buttons that will be in the North on the GUI.*/
    private JButton[] myNorthButtons;
    /** Array of buttons that will be in the South on the GUI.*/
    private JButton[] mySouthButtons;
    /** The frame for this application's GUI.*/
    private final JFrame myFrame;
    /** The label containing the image that is opened by the user.*/
    private final JLabel myImageLabel;
    /** The image user opens for editing.*/
    private PixelImage myImage;
    /** Array of strings with names for the buttons that will be on the top in the GUI.*/
    private final String[] myNButtonStrings;
    /** Array of strings with names for the buttons that will be on the bottom in the GUI.*/
    private final String[] mySButtonStrings;
    
    /**
     * Constructor to initialize fields.
     */
    public SnapShopGUI() {
        myFrame = new JFrame();
        myImageLabel = new JLabel();
        myNButtonStrings = new String[] {"Edge Detect", "Edge Highlight", "Flip Horizontal",
                                         "Flip Vertical", "Grayscale", "Sharpen", "Soften"};
        mySButtonStrings = new String[] {"Open...", "Save As...", "Close Image"};
        myNorthButtons = new JButton[NUM_OF_NORTH_BUTTONS];
        mySouthButtons = new JButton[NUM_OF_SOUTH_BUTTONS];
        myImageOpen = false;
        myChooser = new JFileChooser();
    }
         
    /**
     * Sets up and displays the GUI for this application.
     */
    public void start() {
        myFrame.setTitle("TCSS 305 SnapShop");
        myFrame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        myChooser.setCurrentDirectory(new File(System.getProperty("user.dir")));
        createPanels();
        myFrame.pack();
        myFrame.setMinimumSize(myFrame.getSize());
        myFrame.setVisible(true);
    }
    
    /**
     * Creates the North and South panels with buttons for the GUI.
     */
    private void createPanels() {
        final JPanel panel1 = new JPanel();
        for (int i = 0; i < myNorthButtons.length; i++) {
            myNorthButtons[i] = createButton(myNButtonStrings[i]);
        }
        for (final JButton b : myNorthButtons) {
            panel1.add(b);
        }        
        myFrame.add(panel1, BorderLayout.NORTH);
        
        final JPanel panel2 = new JPanel();
        for (int i = 0; i < mySouthButtons.length; i++) {
            mySouthButtons[i] = createButton(mySButtonStrings[i]);
        }
        for (final JButton b : mySouthButtons) {
            panel2.add(b);
        }
        myFrame.add(panel2, BorderLayout.SOUTH);
    }
    
    /**
     * Creates and returns a button with the name provided. Also contains the ActionListener
     * class that handles ActionEvents for buttons.
     * 
     * @param theString Name of the button.
     * @return button Created button for the GUI.
     */
    private JButton createButton(final String theString) {
        final JButton button = new JButton(theString);
        /** Inner class: ActionListener for Buttons.*/
        class MyActionListener implements ActionListener {
            @Override
            public void actionPerformed(final ActionEvent theEvent) {
                if (theEvent.getSource() == mySouthButtons[0]) {
                    openButton();
                    if (myImageOpen) {
                        for (final JButton b : myNorthButtons) {
                            b.setEnabled(true);
                        }
                        for (final JButton b : mySouthButtons) {
                            b.setEnabled(true);
                        }
                    }
                } else if (theEvent.getSource() == mySouthButtons[1]) {
                    saveButton();
                } else if (theEvent.getSource() == mySouthButtons[2]) {
                    closeImageButton();
                } else {
                    northButtons(theEvent);
                }
            }
        }
        if (!button.getActionCommand().equals(mySButtonStrings[0])) {
            button.setEnabled(false);
        }
        button.addActionListener(new MyActionListener());
        return button;
    }
    
    /**
     * Handles the JFileChooser for whenever the open button is clicked. Opens an open dialog
     * and opens the file if it is an image.
     * 
     * @throws IOException if file is not an image file.
     */
    private void openButton() {
        final int result = myChooser.showOpenDialog(null);
        if (result == JFileChooser.APPROVE_OPTION) {
            try {
                myImage = PixelImage.load(myChooser.getSelectedFile());
                
                myImageLabel.setIcon(new ImageIcon(myImage));
                myImageLabel.setHorizontalAlignment(0);
                myFrame.add(myImageLabel, BorderLayout.CENTER);
                myFrame.setMinimumSize(null);
                myFrame.pack();
                myFrame.setMinimumSize(myFrame.getSize());
                myImageOpen = true;
                myChooser.setCurrentDirectory(myChooser.getCurrentDirectory());
            } catch (final IOException e) {
                JOptionPane.showMessageDialog(null, "The selected file did not contain an " 
                                            + "image!", "Error!", JOptionPane.ERROR_MESSAGE);
            }
        } else if (result == JFileChooser.CANCEL_OPTION) {
            return;
        }
    }
    
    /**
     * Handles the JFileChooser for whenever the save as button is clicked. Opens a save
     * dialog and saves the image.
     * 
     * @throws IOException if the file can not be saved.
     */
    private void saveButton() {
        final int result = myChooser.showSaveDialog(myFrame);
        if (result == JFileChooser.APPROVE_OPTION) {
            final File saveFile = myChooser.getSelectedFile();
            try {
                myImage.save(saveFile);
            } catch (final IOException e) {
                JOptionPane.showMessageDialog(null, "Couldn't save image!", "Save Failed!",
                                              JOptionPane.ERROR_MESSAGE);
            }
        } else if (result == JFileChooser.CANCEL_OPTION) {
            return;
        }
    }
    
    /**
     * Handles the close image button for whenever the button is clicked. Closes the image
     * in the GUI and resizes the GUI back to the original size.
     */
    private void closeImageButton() {
        myImageLabel.setIcon(null);
        myFrame.setMinimumSize(null);
        myFrame.pack();
        myFrame.setMinimumSize(myFrame.getSize());
        myImageOpen = false;
        for (final JButton b : myNorthButtons) {
            b.setEnabled(false);
        }
        for (final JButton b : mySouthButtons) {
            if (!b.getActionCommand().equals(mySButtonStrings[0])) {
                b.setEnabled(false);
            }
        }
    }
    
    /**
     * Handles the buttons that will be placed in the north of the GUI for the ActionListener.
     * 
     * @param theEvent ActionEvent from the ActionListener for the button that is clicked.
     */
    private void northButtons(final ActionEvent theEvent) {
        if (theEvent.getSource() == myNorthButtons[0]) {
            new EdgeDetectFilter().filter(myImage);
        } else if (theEvent.getSource() == myNorthButtons[1]) {
            new EdgeHighlightFilter().filter(myImage);
        } else if (theEvent.getSource() == myNorthButtons[2]) {
            new FlipHorizontalFilter().filter(myImage);
        } else if (theEvent.getSource() == myNorthButtons[3]) {
            new FlipVerticalFilter().filter(myImage);
        } else if (theEvent.getSource() == myNorthButtons[4]) {
            new GrayscaleFilter().filter(myImage);
        } else if (theEvent.getSource() == myNorthButtons[5]) {
            new SharpenFilter().filter(myImage);
        } else if (theEvent.getSource() == myNorthButtons[6]) {
            new SoftenFilter().filter(myImage);
        }
        myImageLabel.setIcon(new ImageIcon(myImage));
    }
}