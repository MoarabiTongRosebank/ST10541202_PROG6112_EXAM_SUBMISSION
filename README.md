# ST10541202_PROG6112_EXAM_SUBMISSION
QUESTION 1
package com.mycompany.st10541202_prog6112_exam;

import java.util.Scanner;

public class ST10541202_PROG6112_EXAM {

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        // --- Get number of products ---
        System.out.print("How many products? ");
        int numProducts = Integer.parseInt(scanner.nextLine().trim());

        // --- Get product names ---
        String[] products = new String[numProducts];
        for (int i = 0; i < numProducts; i++) {
            System.out.print("Enter name for product " + (i + 1) + ": ");
            products[i] = scanner.nextLine().trim();
        }

        // --- Get number of months ---
        System.out.print("How many months of sales data? ");
        int numMonths = Integer.parseInt(scanner.nextLine().trim());

        // --- Get sales data: loop by product, then by month ---
        // sales[productIndex][monthIndex]
        int[][] sales = new int[numProducts][numMonths];
        for (int p = 0; p < numProducts; p++) {
            System.out.println();
            for (int m = 0; m < numMonths; m++) {
                System.out.print("Enter the number of sales for month " + (m + 1)
                        + " for " + products[p] + ": ");
                sales[p][m] = Integer.parseInt(scanner.nextLine().trim());
            }
        }

        // --- Run logic ---
        Product product = new Product(products, sales);
        PrintProduct printer = new PrintProduct();

        System.out.println();
        printer.printTotalProductSales(product.getTotalProductsSold());
        printer.printAverageProductSales(product.getAverageProductSold());
        printer.printMostPopularSale(product.getMostPopularProduct());

        scanner.close();
    }
}

interface IProducts {
    int getTotalProductsSold();
    double getAverageProductSold();
    String getMostPopularProduct();
}

class Product implements IProducts {
    private final String[] products;
    private final int[][] sales;   // sales[productIndex][monthIndex]

    public Product(String[] products, int[][] sales) {
        this.products = products;
        this.sales = sales;
    }

    @Override
    public int getTotalProductsSold() {
        int total = 0;
        for (int[] productSales : sales)
            for (int qty : productSales) total += qty;
        return total;
    }

    @Override
    public double getAverageProductSold() {
        return (double) getTotalProductsSold() / products.length;
    }

    @Override
    public String getMostPopularProduct() {
        int[] totals = new int[products.length];
        for (int p = 0; p < sales.length; p++)
            for (int m = 0; m < sales[p].length; m++) totals[p] += sales[p][m];
        int max = 0;
        for (int i = 1; i < totals.length; i++) if (totals[i] > totals[max]) max = i;
        return products[max];
    }
}

class PrintProduct {
    public void printTotalProductSales(int total) {
        System.out.println("Total Products Sold: " + total);
    }

    public void printAverageProductSales(double average) {
        System.out.printf("Average Products Sold per Product: %.2f%n", average);
    }

    public void printMostPopularSale(String product) {
        System.out.println("Most Popular Product: " + product);
    }
}

class ProductTest {
    static void GetTotalProductSales_WithValidData_ReturnsCorrectValue() {
        Product p = new Product(
            new String[]{"Memory Foam", "Spring", "Hybrid"},
            new int[][]{{10, 20, 30}, {40, 50, 60}, {70, 80, 90}});
        int expected = 450;
        System.out.println("Test 1: " + (p.getTotalProductsSold() == expected ? "PASSED" : "FAILED"));
    }

    static void GetAverageSales_WithValidData_ReturnsCorrectValue() {
        Product p = new Product(
            new String[]{"Memory Foam", "Spring", "Hybrid"},
            new int[][]{{10, 20, 30}, {40, 50, 60}, {70, 80, 90}});
        double expected = 150.0;
        System.out.println("Test 2: " + (Math.abs(p.getAverageProductSold() - expected) < 0.001 ? "PASSED" : "FAILED"));
    }

    public static void main(String[] args) {
        GetTotalProductSales_WithValidData_ReturnsCorrectValue();
        GetAverageSales_WithValidData_ReturnsCorrectValue();
    }
}.  import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

public class CalculatorSimpleTest {

    private Product product;

    @BeforeEach
    public void setUp() {
        product = new Product(new int[]{10, 20, 30});
    }

    @Test
    public void GetTotalProductSales_WithValidData_ReturnsCorrectValue() {
        assertEquals(60, product.GetTotalProductsSold());
    }

    @Test
    public void GetAverageSales_WithValidData_ReturnsCorrectValue() {
        assertEquals(20.0, product.GetAverageSales(), 0.001);
    }
}

class Product {

    private final int[] sales;

    public Product(int[] sales) {
        this.sales = sales;
    }

    public int GetTotalProductsSold() {
        int total = 0;
        for (int s : sales) total += s;
        return total;
    }

    public double GetAverageSales() {
        if (sales == null || sales.length == 0) return 0;
        return (double) GetTotalProductsSold() / sales.length;
    }
}

QUESTION 2
package com.mycompany.st10541202_prog6112_exam_question_2;

import javax.swing.*;
import java.awt.*;
import java.io.FileWriter;
import java.io.IOException;
import java.io.PrintWriter;
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;

// INTERFACE: IProductCalculations

interface IProductCalculations {
    double GetVat();
    double GetCommision();
    double GetTotal();
}

// CLASS: ProductCalculations

class ProductCalculations implements IProductCalculations {

    private double productPrice;

    private static final double VAT_RATE = 0.15;
    private static final double COMMISSION_RATE = 0.05;

    public ProductCalculations(double productPrice) {
        this.productPrice = productPrice;
    }

    public void setProductPrice(double productPrice) {
        this.productPrice = productPrice;
    }

    public double getProductPrice() {
        return productPrice;
    }

    @Override
    public double GetVat() {
        return productPrice * VAT_RATE;
    }

    @Override
    public double GetCommision() {
        return productPrice * COMMISSION_RATE;
    }

    @Override
    public double GetTotal() {
        return productPrice + GetVat() + GetCommision();
    }
}

// MAIN GUI CLASS

public class ST10541202_PROG6112_EXAM_Question_2 extends JFrame {

    private JTextField txtCustomerName;
    private JTextField txtProductPrice;
    private JComboBox<String> cmbProductName;

    private JTextField txtVat;
    private JTextField txtCommission;
    private JTextField txtTotalSale;

    private JButton btnProcess;
    private JButton btnClear;

    private JMenuItem miExit;
    private JMenuItem miProcess;
    private JMenuItem miClear;

    private final String[] PRODUCTS = {
            "Cozy Nights Monarch",
            "Restonic Barbados",
            "Sealy Phuket"
    };

    public ST10541202_PROG6112_EXAM_Question_2() {
        setTitle("Mattress Sales Manager");
        setSize(500, 400);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLocationRelativeTo(null);
        setLayout(new BorderLayout());

        buildMenu();
        buildForm();

        setVisible(true);
    }

    private void buildMenu() {
        JMenuBar menuBar = new JMenuBar();

        JMenu fileMenu = new JMenu("File");
        miExit = new JMenuItem("Exit");
        miExit.addActionListener(e -> System.exit(0));
        fileMenu.add(miExit);

        JMenu toolsMenu = new JMenu("Tools");
        miProcess = new JMenuItem("Process");
        miClear = new JMenuItem("Clear");

        miProcess.addActionListener(e -> processSale());
        miClear.addActionListener(e -> clearForm());

        toolsMenu.add(miProcess);
        toolsMenu.add(miClear);

        menuBar.add(fileMenu);
        menuBar.add(toolsMenu);
        setJMenuBar(menuBar);
    }

    private void buildForm() {
        JPanel formPanel = new JPanel(new GridLayout(6, 2, 8, 8));
        formPanel.setBorder(BorderFactory.createEmptyBorder(15, 15, 15, 15));

        formPanel.add(new JLabel("Customer Name:"));
        txtCustomerName = new JTextField();
        formPanel.add(txtCustomerName);

        formPanel.add(new JLabel("Product Name:"));
        cmbProductName = new JComboBox<>(PRODUCTS);
        formPanel.add(cmbProductName);

        formPanel.add(new JLabel("Product Price:"));
        txtProductPrice = new JTextField();
        formPanel.add(txtProductPrice);

        formPanel.add(new JLabel("VAT Amount (15%):"));
        txtVat = new JTextField();
        txtVat.setEditable(false);
        formPanel.add(txtVat);

        formPanel.add(new JLabel("Commission Amount (5%):"));
        txtCommission = new JTextField();
        txtCommission.setEditable(false);
        formPanel.add(txtCommission);

        formPanel.add(new JLabel("Total Sale Amount:"));
        txtTotalSale = new JTextField();
        txtTotalSale.setEditable(false);
        formPanel.add(txtTotalSale);

        add(formPanel, BorderLayout.CENTER);

        JPanel buttonPanel = new JPanel(new FlowLayout(FlowLayout.CENTER, 10, 10));
        btnProcess = new JButton("Process");
        btnClear = new JButton("Clear");

        btnProcess.addActionListener(e -> processSale());
        btnClear.addActionListener(e -> clearForm());

        buttonPanel.add(btnProcess);
        buttonPanel.add(btnClear);
        add(buttonPanel, BorderLayout.SOUTH);
    }

    private void processSale() {
        String customerName = txtCustomerName.getText().trim();
        String productName = (String) cmbProductName.getSelectedItem();
        String priceText = txtProductPrice.getText().trim();

        if (customerName.isEmpty()) {
            JOptionPane.showMessageDialog(this,
                    "Please enter the customer's name.",
                    "Input Error", JOptionPane.ERROR_MESSAGE);
            return;
        }
        if (priceText.isEmpty()) {
            JOptionPane.showMessageDialog(this,
                    "Please enter the product price.",
                    "Input Error", JOptionPane.ERROR_MESSAGE);
            return;
        }

        double price;
        try {
            price = Double.parseDouble(priceText);
            if (price < 0) throw new NumberFormatException();
        } catch (NumberFormatException ex) {
            JOptionPane.showMessageDialog(this,
                    "Please enter a valid positive number for the product price.",
                    "Input Error", JOptionPane.ERROR_MESSAGE);
            return;
        }

        ProductCalculations calc = new ProductCalculations(price);
        double vat = calc.GetVat();
        double commission = calc.GetCommision();
        double total = calc.GetTotal();

        txtVat.setText(String.format("%.2f", vat));
        txtCommission.setText(String.format("%.2f", commission));
        txtTotalSale.setText(String.format("%.2f", total));

        saveToFile(customerName, productName, price, vat, commission, total);

        JOptionPane.showMessageDialog(this,
                "Sale processed and saved to data.txt successfully.",
                "Success", JOptionPane.INFORMATION_MESSAGE);
    }

    private void saveToFile(String customerName, String productName,
                            double price, double vat, double commission, double total) {
        String timestamp = LocalDateTime.now()
                .format(DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss"));

        try (PrintWriter writer = new PrintWriter(new FileWriter("data.txt", true))) {
            writer.println("========================================");
            writer.println("Date/Time       : " + timestamp);
            writer.println("Customer Name   : " + customerName);
            writer.println("Product Name    : " + productName);
            writer.printf("Product Price   : R %.2f%n", price);
            writer.printf("VAT (15%%)       : R %.2f%n", vat);
            writer.printf("Commission (5%%) : R %.2f%n", commission);
            writer.printf("Total Sale      : R %.2f%n", total);
            writer.println("========================================");
        } catch (IOException e) {
            JOptionPane.showMessageDialog(this,
                    "Error saving to file: " + e.getMessage(),
                    "File Error", JOptionPane.ERROR_MESSAGE);
        }
    }

    private void clearForm() {
        txtCustomerName.setText("");
        txtProductPrice.setText("");
        txtVat.setText("");
        txtCommission.setText("");
        txtTotalSale.setText("");
        cmbProductName.setSelectedIndex(0);
        txtCustomerName.requestFocus();
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> new ST10541202_PROG6112_EXAM_Question_2());
    }
}. package com.mycompany.st10541202_prog6112_exam_question_2;

import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

public class ProductCalculationsTest {

    /**
     * Purpose: Verify that the GetVat method in the ProductCalculations
     * class correctly returns the VAT amount (15% of product price).
     */
    @Test
    public void GetVat_WithValidData_ReturnsCorrectValue() {
        // Arrange
        double price = 1000.00;
        ProductCalculations calc = new ProductCalculations(price);
        double expectedVat = 150.00;   // 15% of 1000

        // Act
        double actualVat = calc.GetVat();

        // Assert
        assertEquals(expectedVat, actualVat, 0.001,
                "VAT should be 15% of the product price");
    }

    /**
     * Purpose: Verify that the GetTotal method in the ProductCalculations
     * class correctly returns the Total Sale Amount
     * (Price + VAT + Commission).
     */
    @Test
    public void GetTotal_WithValidData_ReturnsCorrectValue() {
        // Arrange
        double price = 1000.00;
        ProductCalculations calc = new ProductCalculations(price);
        // Expected: 1000 + 150 (VAT) + 50 (Commission) = 1200
        double expectedTotal = 1200.00;

        // Act
        double actualTotal = calc.GetTotal();

        // Assert
        assertEquals(expectedTotal, actualTotal, 0.001,
                "Total should equal Price + VAT + Commission");
    }
}
