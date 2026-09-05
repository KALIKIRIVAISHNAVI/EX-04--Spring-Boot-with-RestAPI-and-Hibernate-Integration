# Exp-04-Spring-Boot-with-REST-API-and-Hibernate-Integration

# Name: KALIKIRI VAISHNAVI
# Register Number: 212223040081

## AIM:
To develop a Spring Boot application to store and retrieve data from a Movies database using Object Relational Mapping (ORM) with Hibernate and expose it via REST APIs.

## ALGORITHM:
Create Spring Boot project with dependencies:

Spring Web

Spring Data JPA

H2 or MySQL Database

Configure application.properties with DB connection and JPA settings.

Create Movie entity with fields like id, title, genre, rating, and year.

Create MovieRepository interface extending JpaRepository.

Create MovieController to define REST endpoints for CRUD operations:

GET /movies

GET /movies/{id}

POST /movies

PUT /movies/{id}

DELETE /movies/{id}


## PROGRAM CODE (Main Files):
### application.properties
```
spring.application.name=ex4

spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=

spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true

spring.h2.console.enabled=true
spring.h2.console.path=/h2-console

server.port=8081
```

### Movie.java
```
package com.example.ex4;

import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.GenerationType;
import jakarta.persistence.Id;

@Entity
public class Movie {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String title;
    private String genre;
    private double rating;
    private int releaseYear;

    public Movie() {
    }

    public Movie(String title, String genre, double rating, int releaseYear) {
        this.title = title;
        this.genre = genre;
        this.rating = rating;
        this.releaseYear = releaseYear;
    }

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getTitle() {
        return title;
    }

    public void setTitle(String title) {
        this.title = title;
    }

    public String getGenre() {
        return genre;
    }

    public void setGenre(String genre) {
        this.genre = genre;
    }

    public double getRating() {
        return rating;
    }

    public void setRating(double rating) {
        this.rating = rating;
    }

    public int getReleaseYear() {
        return releaseYear;
    }

    public void setReleaseYear(int releaseYear) {
        this.releaseYear = releaseYear;
    }
}
```
### MovieRepository.java
```
package com.example.ex4;

import org.springframework.data.jpa.repository.JpaRepository;

public interface MovieRepository extends JpaRepository<Movie, Long> {
}
```
### MovieController.java
```
package com.example.ex4;

import org.springframework.web.bind.annotation.*;
import java.util.List;

@RestController
@RequestMapping("/movies")
public class MovieController {

    private final MovieRepository movieRepository;

    // Constructor Injection
    public MovieController(MovieRepository movieRepository) {
        this.movieRepository = movieRepository;
    }

    // 1. GET /movies
    // Get all movies
    @GetMapping
    public List<Movie> getAllMovies() {
        return movieRepository.findAll();
    }

    // 2. GET /movies/{id}
    // Get one movie by ID
    @GetMapping("/{id}")
    public Movie getMovieById(@PathVariable Long id) {
        return movieRepository.findById(id).orElse(null);
    }

    // 3. POST /movies
    // Add a new movie
    @PostMapping
    public Movie addMovie(@RequestBody Movie movie) {
        return movieRepository.save(movie);
    }

    // 4. PUT /movies/{id}
    // Update an existing movie
    @PutMapping("/{id}")
    public Movie updateMovie(
            @PathVariable Long id,
            @RequestBody Movie movie) {

        Movie existingMovie = movieRepository.findById(id).orElse(null);

        if (existingMovie != null) {
            existingMovie.setTitle(movie.getTitle());
            existingMovie.setGenre(movie.getGenre());
            existingMovie.setRating(movie.getRating());
            existingMovie.setReleaseYear(movie.getReleaseYear());

            return movieRepository.save(existingMovie);
        }

        return null;
    }

    // 5. DELETE /movies/{id}
    // Delete a movie
    @DeleteMapping("/{id}")
    public String deleteMovie(@PathVariable Long id) {
        movieRepository.deleteById(id);
        return "Movie deleted successfully";
    }
}
```

# EX4Application.java
```
package com.example.ex4;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class Ex4Application {

	public static void main(String[] args) {
		SpringApplication.run(Ex4Application.class, args);
	}

}

```

# Output:

## POST

<img width="1535" height="863" alt="ex4 post" src="https://github.com/user-attachments/assets/e2a6b154-3be0-402c-84ec-743aaaafa2c3" />


## GET


<img width="1535" height="863" alt="ex4 get" src="https://github.com/user-attachments/assets/d48d3d5f-4179-478f-8720-de02af99240d" />


## GET BY ID


<img width="1535" height="863" alt="ex4 get id" src="https://github.com/user-attachments/assets/4f9d3d46-7fdf-4047-ae3a-ed09da23ecd7" />


## PUT


<img width="1530" height="863" alt="ex4 put" src="https://github.com/user-attachments/assets/5dc79af5-ffa5-4bb3-8211-c3cf8332be2f" />


## DELETE


<img width="1535" height="863" alt="ex4 delete" src="https://github.com/user-attachments/assets/9f084d05-1074-421c-b74c-c789bc0d54c7" />



# Result:

The output for this lab experiment confirms that all CRUD operations function properly on the H2 database

