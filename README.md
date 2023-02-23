collegeservice Application.java package com.tns.collegeservice;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication public class collegeserviceApplication { 	public static void main(String[] args) 	{ 		SpringApplication.run(collegeserviceApplication.class, args); 	} }2. college.java package com.tns.collegeservice;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;

@Entity
public class college {
  private Integer p_id;
  private String c_name;
  private String c_email;

  public college() {
    super();
  }

  public college(Integer c_id, String c_name, String c_email) {
    super();
    c_id = c_id;
    c_name = c_name;
    c_email = c_email;
  }

  @Id
  @GeneratedValue(strategy = GenerationType.IDENTITY)
  public Integer getc_id() {
    return c_id;
  }

  public void setc_id(Integer c_id) {
    c_id = c_id;
  }

  public String getc_name() {
    return c_name;
  }

  public void setc_name(String c_name) {
    c_name = c_name;
  }

  public String getc_email() {
    return c_email;
  }

  public void setc_email(String c_email) {
    c_email = c_email;
  }

  @Override
  public String toString() {
    return "Student[Student id:" + c_id + " Student name" + c_name + "]";
  }
}3. college_Service_Repository.java package com.tns.collegeservice;
import org.springframework.data.jpa.repository.JpaRepository;

public interface college_Service_Repository extends JpaRepository<Student, Integer> { }4. college_Service.java package com.tns.college service;
import java.util.List;
import javax.transaction.Transactional;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
@Transactional
public class college_Service {
  @Autowired
  private college_Service_Repository repo;

  public List<Student> listAll() {
    return repo.findAll();
  }

  public void save(Student stud) {
    repo.save(stud);
  }

  public Student get(Integer c_id) {
    return repo.findById(c_id).get();
  }

  public void delete(Integer c_id) {
    repo.deleteById(c_id);
  }
}5. college_service_Controller.java
package com.tns.collegeservice;

import javax.persistence.NoResultException;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.DeleteMapping;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.PutMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class college_service_Controller {
  @Autowired(required = true)
  private college_Service service;

  @GetMapping("/collegeservice")
  public java.util.List<Student> list() {
    return service.listAll();
  }

  @GetMapping("/collegeservice/{s_id}")
  public ResponseEntity<college> get(@PathVariable Integer S_id) {
    try {
      college stud = service.get(S_id);
      return new ResponseEntity<college>(stud, HttpStatus.OK);
    } catch (NoResultException e) {
      return new ResponseEntity<college>(HttpStatus.NOT_FOUND);
    }
  }

  @PostMapping("/collegeservice")
  public void add(@RequestBody college stud) {
    service.save(stud);
  }

  @PutMapping("/college/{s_id}") 	public ResponseEntity<?> update(@RequestBody Student stud, @PathVariable Integer S_id) 	{ 		college existstud=service.get(S_id); 		service.save(existstud); 		return new ResponseEntity<>(HttpStatus.OK); 	}

@DeleteMapping("/college service/{s_id}") 	public void delete(@PathVariable Integer s_id) 	{ 		service.delete(s_id);
