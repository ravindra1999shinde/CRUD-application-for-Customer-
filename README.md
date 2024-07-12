# CRUD-application-for-Customer-


The Create a CRUD application for Customer

spring.datasource.url=jdbc:mysql://localhost:3306/your_database
spring.datasource.username=your_username
spring.datasource.password=your_password
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL5Dialect


@Entity
public class Customer {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String firstName;
    private String lastName;
    private String street;
    private String address;
    private String city;
    private String state;
    private String email;
    private String phone;

    // Getters and Setters
}


public interface CustomerRepository extends JpaRepository<Customer, Long> {
    Page<Customer> findAll(Pageable pageable);
    List<Customer> findByFirstNameContainingOrLastNameContaining(String firstName, String lastName);
}


@Service
public class CustomerService {
    @Autowired
    private CustomerRepository customerRepository;

    // CRUD operations
}


@RestController
@RequestMapping("/api/customers")
public class CustomerController {
    @Autowired
    private CustomerService customerService;

    // Define API endpoints for CRUD operations
}


public String authenticateWithRemoteAPI() {
    // Use RestTemplate or WebClient to make a POST request to the remote API for authentication
}


public List<Customer> fetchCustomersFromRemoteAPI(String token) {
    // Use RestTemplate or WebClient to make a GET request to the remote API
}


@RestController
@RequestMapping("/api/customers")
public class CustomerController {
    @Autowired
    private CustomerService customerService;

    @GetMapping
    public Page<Customer> getAllCustomers(@RequestParam int page, @RequestParam int size) {
        return customerService.getAllCustomers(PageRequest.of(page, size));
    }

    @GetMapping("/{id}")
    public ResponseEntity<Customer> getCustomerById(@PathVariable Long id) {
        return customerService.getCustomerById(id);
    }

    @PostMapping
    public Customer createCustomer(@RequestBody Customer customer) {
        return customerService.createCustomer(customer);
    }

    @PutMapping("/{id}")
    public ResponseEntity<Customer> updateCustomer(@PathVariable Long id, @RequestBody Customer customer) {
        return customerService.updateCustomer(id, customer);
    }

    @DeleteMapping("/{id}")
    public void deleteCustomer(@PathVariable Long id) {
        customerService.deleteCustomer(id);
    }
}


@Service
public class CustomerService {
    @Autowired
    private CustomerRepository customerRepository;

    public Page<Customer> getAllCustomers(Pageable pageable) {
        return customerRepository.findAll(pageable);
    }

    public ResponseEntity<Customer> getCustomerById(Long id) {
        Optional<Customer> customer = customerRepository.findById(id);
        return customer.isPresent() ? ResponseEntity.ok(customer.get()) : ResponseEntity.notFound().build();
    }

    public Customer createCustomer(Customer customer) {
        return customerRepository.save(customer);
    }

    public ResponseEntity<Customer> updateCustomer(Long id, Customer customer) {
        if (!customerRepository.existsById(id)) {
            return ResponseEntity.notFound().build();
        }
        customer.setId(id);
        return ResponseEntity.ok(customerRepository.save(customer));
    }

    public void deleteCustomer(Long id) {
        customerRepository.deleteById(id);
    }
}


<!DOCTYPE html>
<html>
<head>
    <title>Customer Management</title>
</head>
<body>
    <h1>Customer Management</h1>
    <div id="customerList"></div>
    <form id="customerForm">
        <!-- Form fields for customer details -->
        <button type="submit">Save</button>
    </form>

    <script src="app.js"></script>
</body>
</html>


document.addEventListener('DOMContentLoaded', function() {
    // Fetch customers and display in the list
});

document.getElementById('customerForm').addEventListener('submit', function(e) {
    e.preventDefault();
    // Save customer via API call
});
