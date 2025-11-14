
CREATE DATABASE ElectionDB;
USE ElectionDB;

CREATE TABLE States (
    state_id INT PRIMARY KEY AUTO_INCREMENT,
    state_name VARCHAR(100) UNIQUE NOT NULL
);

CREATE TABLE Districts (
    district_id INT PRIMARY KEY AUTO_INCREMENT,
    district_name VARCHAR(100) NOT NULL,
    state_id INT NOT NULL,
    FOREIGN KEY (state_id) REFERENCES States(state_id)
);

CREATE TABLE Booths (
    booth_id INT PRIMARY KEY AUTO_INCREMENT,
    booth_name VARCHAR(100) NOT NULL,
    district_id INT NOT NULL,
    voters_capacity INT CHECK (voters_capacity > 0),
    FOREIGN KEY (district_id) REFERENCES Districts(district_id)
);

CREATE TABLE Voters (
    voter_id INT PRIMARY KEY AUTO_INCREMENT,
    voter_name VARCHAR(100) NOT NULL,
    age INT CHECK (age >= 18),
    gender VARCHAR(10),
    booth_id INT NOT NULL,
    district_id INT NOT NULL,
    FOREIGN KEY (booth_id) REFERENCES Booths(booth_id),
    FOREIGN KEY (district_id) REFERENCES Districts(district_id)
);

CREATE TABLE Parties (
    party_id INT PRIMARY KEY AUTO_INCREMENT,
    party_name VARCHAR(100) UNIQUE,
    symbol VARCHAR(50)
);

CREATE TABLE Candidates (
    candidate_id INT PRIMARY KEY AUTO_INCREMENT,
    candidate_name VARCHAR(100) NOT NULL,
    party_id INT NOT NULL,
    district_id INT NOT NULL,
    FOREIGN KEY (party_id) REFERENCES Parties(party_id),
    FOREIGN KEY (district_id) REFERENCES Districts(district_id)
);

CREATE TABLE Votes (
    vote_id INT PRIMARY KEY AUTO_INCREMENT,
    voter_id INT UNIQUE NOT NULL,
    candidate_id INT NOT NULL,
    booth_id INT NOT NULL,
    vote_time TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (voter_id) REFERENCES Voters(voter_id),
    FOREIGN KEY (candidate_id) REFERENCES Candidates(candidate_id),
    FOREIGN KEY (booth_id) REFERENCES Booths(booth_id)
);
