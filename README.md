# cơ sở dữ liệu H2 database, spring boot với h2 database

## Cơ sở dữ liệu h2 database 

H2 database là một hệ quản trị cơ sở dữ liệu quan hệ (Relational Database Management System - RDBMS) mã nguồn mở được viết bằng Java. Nó cung cấp các tính năng tiêu chuẩn của cơ sở dữ liệu quan hệ như tạo, dừng, truy vấn và quản lý dữ liệu.

### 1  Cách cài đặt H2 database:

- Bước 1: Tải xuống H2 database từ trang web chính thức của nó [http://www.h2database.com/html/main.html](http://www.h2database.com/html/main.html).
- Bước 2: Giải nén tệp tải xuống và lưu trữ nó trong một thư mục trên máy tính của bạn.
- Bước 3: Chạy tệp jar của H2 bằng cách nhấp đúp vào nó. Một giao diện người dùng web sẽ xuất hiện trên trình

### Cách sử dụng H2 database:

- Bước 1: Thiết lập kết nối với cơ sở dữ liệu H2. Bạn có thể sử dụng JDBC (Java Database Connectivity) để kết nối với H2 database trong các ứng dụng Java của mình.
- Bước 2: Tạo database và bảng: Bạn có thể sử dụng SQL để tạo cơ sở dữ liệu và bảng trong H2 database.
- Bước 3: Truy vấn và quản lý dữ liệu: Bạn có thể sử dụng SQL để thực hiện các truy vấn và quản lý dữ liệu trong cơ sở dữ liệu H2.

### Tính năng của H2 database:

- Hỗ trợ chuẩn SQL: H2 database hỗ trợ hầu hết các câu lệnh và chức năng của SQL, cho phép bạn thực hiện các truy vấn phức tạp và quản lý dữ liệu một cách linh hoạt.
- Hỗ trợ mã hóa và bảo mật: H2 database cung cấp các tính năng mã hóa và bảo mật để bảo vệ dữ liệu của bạn khỏi các mối đe dọa bảo mật.
- Tương thích đa nền tảng: H2 database có thể chạy trên nhiều nền tảng khác nhau như Windows, Linux và macOS, đảm bảo tính tương thích và di động cao.


## spring boot với h2 database

### 1: Khởi tạo project sprig boot
### 2: cấu hình dependencies

Để sử dụng cơ sở dữ liệu H2 trong ứng dụng spring boot, chúng ta phải thêm phần phụ thuộc sau vào file pom.xml:

```
dependency>
      <groupId>com.h2database</groupId>
      <artifactId>h2</artifactId>
      <scope>runtime</scope>
 </dependency>

```

### 3: Database Configuration

Mặc định, Spring Boot sẽ cấu hình ứng dụng để kết nối với kho lưu trữ trong bộ nhớ với tên người dùng là: sa và mật khẩu trống.
Tuy nhiên, chúng ta có thể thay đổi các tham số đó bằng cách thêm các thuộc tính sau vào file application.properties:

```
spring.h2.console.enabled=true
spring.datasource.url=jdbc:h2:mem:dcbapp
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=password
spring.jpa.database-platform=org.hibernate.dialect.H2Dialect

```

H2 Console Theo mặc định, chế độ xem bảng điều khiển của cơ sở dữ liệu H2 bị tắt. Trước khi truy cập cơ sở dữ liệu H2, chúng ta phải kích hoạt nó bằng cách sử dụng thuộc tính sau.

```
spring.h2.console.enabled=true
```

Khi chúng ta đã bật bảng điều khiển H2, bây giờ chúng tôi có thể truy cập bảng điều khiển H2 trong trình duyệt bằng cách gọi URL http://localhost:8082/h2-console

> Lưu ý: Cung cấp số cổng nơi ứng dụng mùa xuân của bạn đang chạy

![image](https://github.com/thangdtph27626/H2DatabaseWithSpringboot/assets/109157942/f1aa2c28-67b7-4609-b9c3-8c6fab5db2f7)

### 4: Cấu hình các package trong project

Tạo 4 package ghồm: controller, entity, service, serviceImpl, repository


### 5: Entity 

Tạo một lớp trong Department.class trong package entity

```
package com.amiya.springbootdemoproject.entity;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import lombok.AllArgsConstructor;
import lombok.Builder;
import lombok.Data;
import lombok.NoArgsConstructor;

@Entity
@Data
@NoArgsConstructor
@AllArgsConstructor
@Builder
public class Department {

	@Id
	@GeneratedValue(strategy = GenerationType.AUTO)
	private Long departmentId;
	private String departmentName;
	private String departmentAddress;
	private String departmentCode;
}


```

### 5: Repository

Tạo một interface  và đặt tên interface n là DepartmentRepository. interface  sẽ mở rộng JpaRepository như chúng ta đã thảo luận ở trên.

Ví dụ Dưới đây là mã cho tệp DepartmentRepository.java


```
package com.amiya.springbootdemoproject.repository;

import com.amiya.springbootdemoproject.entity.Department;
import org.springframework.data.repository.CrudRepository;
import org.springframework.stereotype.Repository;

@Repository

public interface DepartmentRepository
	extends JpaRepository<Department, Long> {
}

```

### 6: Service

Bên trong package Service, tạo mộtinterface có tên là DepartmentService và một package có tên là ServiceImpl.

```
package com.amiya.springbootdemoproject.service;
import com.amiya.springbootdemoproject.entity.Department;
import java.util.List;

public interface DepartmentService {

	Department saveDepartment(Department department);

	List<Department> fetchDepartmentList();

	Department updateDepartment(Department department,
								Long departmentId);

	void deleteDepartmentById(Long departmentId);
}

```

### 6: ServiceImpl

Khởi tạo một class DepartmentServiceImpl implements từ DepartmentService
```
package com.amiya.springbootdemoproject.service;

import com.amiya.springbootdemoproject.entity.Department;
import com.amiya.springbootdemoproject.repository.DepartmentRepository;
import java.util.List;
import java.util.Objects;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class DepartmentServiceImpl
	implements DepartmentService {

	@Autowired
	private DepartmentRepository departmentRepository;

	@Override
	public Department saveDepartment(Department department)
	{
		return departmentRepository.save(department);
	}

	@Override public List<Department> fetchDepartmentList()
	{
		return (List<Department>)
			departmentRepository.findAll();
	}

	@Override
	public Department
	updateDepartment(Department department,
					Long departmentId)
	{
		Department depDB	= departmentRepository.findById(departmentId).get();
    
    BeanUtils.copyProperties(department, depDB);
	
		return departmentRepository.save(depDB);
	}

	@Override
	public void deleteDepartmentById(Long departmentId)
	{
		departmentRepository.deleteById(departmentId);
	}
}

```


### 7: Controller


Tạo một class DepartmentController 

```
package com.amiya.springbootdemoproject.controller;

import com.amiya.springbootdemoproject.entity.Department;
import com.amiya.springbootdemoproject.service.DepartmentService;
import java.util.List;
import javax.validation.Valid;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

@RestController
public class DepartmentController {

	@Autowired private DepartmentService departmentService;

	@PostMapping("/departments")
	public Department saveDepartment(
		@Valid @RequestBody Department department)
	{
		return departmentService.saveDepartment(department);
	}

	@GetMapping("/departments")
	public List<Department> fetchDepartmentList()
	{
		return departmentService.fetchDepartmentList();
	}

	@PutMapping("/departments/{id}")
	public Department
	updateDepartment(@RequestBody Department department,
					@PathVariable("id") Long departmentId)
	{
		return departmentService.updateDepartment(
			department, departmentId);
	}

	// Delete operation
	@DeleteMapping("/departments/{id}")
	public String deleteDepartmentById(@PathVariable("id")
									Long departmentId)
	{
		departmentService.deleteDepartmentById(
			departmentId);
		return "Deleted Successfully";
	}
}

```


### 8: Kết quả 

1: POST – http://localhost:8080/departments/ 

![image](https://github.com/thangdtph27626/H2DatabaseWithSpringboot/assets/109157942/5f989e9d-4d47-440c-9ac5-e9039d62e22d)


2: GET – http://localhost:8080/departments/ 

![image](https://github.com/thangdtph27626/H2DatabaseWithSpringboot/assets/109157942/adbb7c40-eb2f-4c5b-8ce0-9834c86e7952)

3: PUT – http://localhost:8080/departments/1 

![image](https://github.com/thangdtph27626/H2DatabaseWithSpringboot/assets/109157942/16f85050-43ad-47d2-a1c6-c2ef0a3abefc)

4: DELETE – http://localhost:8080/departments/1 

![image](https://github.com/thangdtph27626/H2DatabaseWithSpringboot/assets/109157942/bdbebd0e-7e85-443b-9e6c-4c1b8794003d)


