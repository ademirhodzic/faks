# competition_database

CREATE TABLE natjecanje(
    natjecanje_id INT AUTO_INCREMENT,
    naziv_natjecanja VARCHAR (45) NOT NULL,
    datum_natjecanja DATE NOT NULL,
    adresa VARCHAR(20) NOT NULL,
    grad VARCHAR(10) NOT NULL,
    opis_natjecanja VARCHAR(300),
    PRIMARY KEY (natjecanje_id)
);

CREATE TABLE disciplina(
    disciplina_id INT AUTO_INCREMENT,
    naziv_discipline VARCHAR(20) NOT NULL,
    PRIMARY KEY (disciplina_id)
);

CREATE TABLE disciplina_natjecanja(
    natjecanje_id INT,
    disciplina_id INT,
    kotizacija NUMERIC (12,2) NOT NULL,
    PRIMARY KEY (natjecanje_id, disciplina_id),
    FOREIGN KEY (disciplina_id) REFERENCES disciplina(disciplina_id),
    FOREIGN KEY (natjecanje_id) REFERENCES natjecanje(natjecanje_id)
);

CREATE TABLE prijava_natjecatelja(
    prijava_id INT AUTO_INCREMENT,
    natjecanje_id INT NOT NULL,
    ime VARCHAR(30) NOT NULL,
    prezime VARCHAR(30) NOT NULL,
    spol CHAR(1) NOT NULL,                   
    OIB CHAR(11) NOT NULL UNIQUE,
    datum_rodenja DATE NOT NULL,
    datum_prijave DATE NOT NULL,
    iznos_kotizacije NUMERIC(12,2),                        
    stanje_uplate CHAR(2) DEFAULT 'ne',    
    PRIMARY KEY (prijava_id),
    FOREIGN KEY (natjecanje_id) REFERENCES natjecanje(natjecanje_id) ON DELETE CASCADE
);

CREATE TABLE stavke_prijave(
    prijava_id INT,
    natjecanje_id INT NOT NULL,
    disciplina_id INT NOT NULL,
    season_best DECIMAL(5) NOT NULL,
    PRIMARY KEY (prijava_id),
    FOREIGN KEY (prijava_id) REFERENCES prijava_natjecatelja(prijava_id) ON DELETE CASCADE,
    FOREIGN KEY (natjecanje_id, disciplina_id) REFERENCES disciplina_natjecanja(natjecanje_id, disciplina_id) ON DELETE CASCADE
);
    
CREATE TABLE status_utrke(
    id INT AUTO_INCREMENT,
    kratica CHAR(3) NOT NULL,
    opis VARCHAR(100),
    PRIMARY KEY(id)
);
    
CREATE TABLE rezultati(
    prijava_id INT,
    rezultat DECIMAL(5) NOT NULL,                     
    status_utrke INT NOT NULL,                            
    PRIMARY KEY (prijava_id),
    FOREIGN KEY (status_utrke) REFERENCES status_utrke(id),
    FOREIGN KEY (prijava_id) REFERENCES stavke_prijave(prijava_id)
);
