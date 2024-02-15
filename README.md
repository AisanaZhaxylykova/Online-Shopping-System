# Online-Shopping-System
import java.sql.*;

public class OnlineShoppingSystem {
    static final String JDBC_URL = "jdbc:postgresql://localhost:5432/OSS";
    static final String USERNAME = "Aisana";
    static final String PASSWORD = "qwerty123";

    public static void main(String[] args) {
        try (Connection connection = DriverManager.getConnection(JDBC_URL, USERNAME, PASSWORD)) {
            Product product = new Product("Laptop", 999.99);
            createProduct(connection, product);
            ResultSet resultSet = getAllProducts(connection);
            while (resultSet.next()) {
                String name = resultSet.getString("name");
                double price = resultSet.getDouble("price");
                System.out.println("Product: " + name + ", Price: $" + price);
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

    static void createProduct(Connection connection, Product product) throws SQLException {
        String sql = "INSERT INTO products (name, price) VALUES (?, ?)";
        try (PreparedStatement statement = connection.prepareStatement(sql)) {
            statement.setString(1, product.getName());
            statement.setDouble(2, product.getPrice());
            statement.executeUpdate();
        }
    }

    static ResultSet getAllProducts(Connection connection) throws SQLException {
        String sql = "SELECT * FROM products";
        Statement statement = connection.createStatement();
        return statement.executeQuery(sql);
    }
}

class Product {
    private String name;
    private double price;

    public Product(String name, double price) {
        this.name = name;
        this.price = price;
    }

    public String getName() {
        return name;
    }

    public double getPrice() {
        return price;
    }
    static void updateProduct(Connection connection, int productId, Product newProductDetails) throws SQLException {
        String sql = "UPDATE products SET name = ?, price = ? WHERE id = ?";
        try (PreparedStatement statement = connection.prepareStatement(sql)) {
            statement.setString(1, newProductDetails.getName());
            statement.setDouble(2, newProductDetails.getPrice());
            statement.setInt(3, productId);
            statement.executeUpdate();
        }
    }
    static void deleteProduct(Connection connection, int productId) throws SQLException {
        String sql = "DELETE FROM products WHERE id = ?";
        try (PreparedStatement statement = connection.prepareStatement(sql)) {
            statement.setInt(1, productId);
            statement.executeUpdate();
        }
    }
}
