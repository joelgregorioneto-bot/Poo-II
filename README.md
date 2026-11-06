# Poo-II

// 1. Herança: Aluno e Professor herdam de Pessoa

public class Exercicio1 {
    public static void main(String[] args) {
        Aluno aluno = new Aluno("Ana");
        Professor professor = new Professor("Carlos");

        // getNome() foi herdado de Pessoa
        System.out.println("Aluno: " + aluno.getNome());
        System.out.println("Professor: " + professor.getNome());
    }
}

class Pessoa {
    protected String nome;

    public Pessoa(String nome) {
        this.nome = nome;
    }

    public String getNome() {
        return nome;
    }
}

class Aluno extends Pessoa {
    public Aluno(String nome) {
        super(nome);
    }
}

class Professor extends Pessoa {
    public Professor(String nome) {
        super(nome);
    }
}

===========================================================

// 2. Encapsulamento: o saldo é privado e só é acessado pelos métodos da classe

public class Exercicio2 {
    public static void main(String[] args) {
        ContaBancaria conta = new ContaBancaria();

        conta.depositar(500);
        conta.sacar(200);
        conta.sacar(1000); // saldo insuficiente

        System.out.println("Saldo atual: R$ " + conta.consultarSaldo());

        // conta.saldo = 1000000; // ERRO de compilação: saldo é private
    }
}

class ContaBancaria {
    private double saldo;

    public void depositar(double valor) {
        if (valor <= 0) {
            System.out.println("Valor de depósito inválido.");
            return;
        }
        saldo += valor;
    }

    public void sacar(double valor) {
        if (valor <= 0) {
            System.out.println("Valor de saque inválido.");
        } else if (valor > saldo) {
            System.out.println("Saldo insuficiente.");
        } else {
            saldo -= valor;
        }
    }

    public double consultarSaldo() {
        return saldo;
    }
}
===================================================
// 3. Polimorfismo: cada animal implementa emitirSom() do seu jeito

public class Exercicio3 {
    public static void main(String[] args) {
        Animal[] animais = { new Cachorro(), new Gato(), new Animal() };

        // A mesma chamada produz resultados diferentes conforme o objeto real
        for (Animal a : animais) {
            a.emitirSom();
        }
    }
}

class Animal {
    public void emitirSom() {
        System.out.println("Som genérico de animal");
    }
}

class Cachorro extends Animal {
    @Override
    public void emitirSom() {
        System.out.println("Au au!");
    }
}

class Gato extends Animal {
    @Override
    public void emitirSom() {
        System.out.println("Miau!");
    }
}
===============================================
// 4. Abstração: Forma define o contrato; as filhas implementam o cálculo

public class Exercicio4 {
    public static void main(String[] args) {
        // Forma forma = new Forma(); // ERRO: classe abstrata não pode ser instanciada
        Forma[] formas = { new Retangulo(4, 5), new Circulo(3) };

        for (Forma f : formas) {
            System.out.printf("Área: %.2f%n", f.calcularArea());
        }
    }
}

abstract class Forma {
    public abstract double calcularArea();
}

class Retangulo extends Forma {
    private double base;
    private double altura;

    public Retangulo(double base, double altura) {
        this.base = base;
        this.altura = altura;
    }

    @Override
    public double calcularArea() {
        return base * altura;
    }
}

class Circulo extends Forma {
    private double raio;

    public Circulo(double raio) {
        this.raio = raio;
    }

    @Override
    public double calcularArea() {
        return Math.PI * raio * raio;
    }
}
========================================
import java.util.ArrayList;
import java.util.List;

public class Exercicio5 {
    public static void main(String[] args) {
        Aluno aluno = new Aluno("Ana", 20, "2024001");
        aluno.adicionarNota(8.5);
        aluno.adicionarNota(9.0);
        aluno.adicionarNota(11); // inválida

        Professor professor = new Professor("Carlos", 45, "Programação Orientada a Objetos", 7500);

        List<Pessoa> pessoas = new ArrayList<>();
        pessoas.add(aluno);
        pessoas.add(professor);

        // Polimorfismo: mesma chamada, comportamentos diferentes
        for (Pessoa p : pessoas) {
            p.apresentar();
        }
    }
}

// Interface: contrato comum (ABSTRAÇÃO)
interface Apresentavel {
    void apresentar();
}

// Classe abstrata base (ABSTRAÇÃO) com atributos privados (ENCAPSULAMENTO)
abstract class Pessoa implements Apresentavel {
    private String nome;
    private int idade;

    public Pessoa(String nome, int idade) {
        this.nome = nome;
        setIdade(idade);
    }

    public String getNome() {
        return nome;
    }

    public int getIdade() {
        return idade;
    }

    public void setIdade(int idade) {
        if (idade < 0) {
            throw new IllegalArgumentException("Idade não pode ser negativa.");
        }
        this.idade = idade;
    }

    // Cada subclasse define como se apresenta
    @Override
    public abstract void apresentar();
}

// HERANÇA: Aluno herda de Pessoa
class Aluno extends Pessoa {
    private String matricula;
    private List<Double> notas = new ArrayList<>();

    public Aluno(String nome, int idade, String matricula) {
        super(nome, idade);
        this.matricula = matricula;
    }

    public void adicionarNota(double nota) {
        if (nota < 0 || nota > 10) {
            System.out.println("Nota inválida: " + nota);
            return;
        }
        notas.add(nota);
    }

    public double calcularMedia() {
        if (notas.isEmpty()) return 0;
        double soma = 0;
        for (double n : notas) soma += n;
        return soma / notas.size();
    }

    // POLIMORFISMO: implementação própria de Aluno
    @Override
    public void apresentar() {
        System.out.printf("Aluno %s, %d anos, matrícula %s, média %.1f%n",
                getNome(), getIdade(), matricula, calcularMedia());
    }
}

// HERANÇA: Professor herda de Pessoa
class Professor extends Pessoa {
    private String disciplina;
    private double salario; // encapsulado

    public Professor(String nome, int idade, String disciplina, double salario) {
        super(nome, idade);
        this.disciplina = disciplina;
        this.salario = salario;
    }

    public double getSalario() {
        return salario;
    }

    // POLIMORFISMO: implementação diferente da de Aluno
    @Override
    public void apresentar() {
        System.out.printf("Professor %s, %d anos, leciona %s%n",
                getNome(), getIdade(), disciplina);
    }
}

