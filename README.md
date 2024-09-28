# banco-em-java
package main;
import modelo.Apartamento;
import modelo.Casa;

import java.io.*;
import java.util.ArrayList;
import java.util.List;
import java.io.Serializable;
import java.io.EOFException;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.ObjectInputStream;
import java.io.ObjectOutputStream;

import modelo.Terreno;
import util.InterfaceUsuario;
import modelo.Financiamento;
import java.text.DecimalFormat;


public class Main  {
    public static void main(String[] args) {
        InterfaceUsuario interfaceUsuario = new InterfaceUsuario();
        List<Financiamento> listaDeFinanciamentos = new ArrayList<>();
        DecimalFormat df = new DecimalFormat("#,###.00");
        DecimalFormat xf = new DecimalFormat("#.###");



        // Coleta de dados de financiamento da casa
        for (int i = 0; i < 2; i++) {
            double taxaJuros = interfaceUsuario.pedirTaxaJuros();
            int prazoFinanciamentoAnos = interfaceUsuario.pedirPrazoFinanciamento();
            double valorImovel = interfaceUsuario.pedirValorImovel();
            double areaconstruida = interfaceUsuario.pedirareaconstruida();
            double areaterreno = interfaceUsuario.pedirareaterreno();
            Financiamento financiamento = new Casa(valorImovel, prazoFinanciamentoAnos, taxaJuros,areaconstruida,areaterreno);
            listaDeFinanciamentos.add(financiamento);


            /*FileWriter escritorcasa = null;

            try {
                escritorcasa = new FileWriter("arquivocasa.txt");
                escritorcasa.write(Casa.toString());

                escritorcasa.flush();
                escritorcasa.close(); // fecha arquivo de saída
            }catch (FileNotFoundException e) {
                e.printStackTrace();
            } catch (IOException e) {
                e.printStackTrace();
            }*/



            // Impressão dos detalhes do financiamento
            System.out.println("Financiamento casa " + (i + 1) + ":");
            System.out.println("Valor do imóvel casa: " + valorImovel);
            System.out.println("Prazo de financiamento da casa: " + prazoFinanciamentoAnos + " anos");
            System.out.println("Taxa de juros da casa: " + taxaJuros + "%");
            System.out.println("area construida : " + areaconstruida);
            System.out.println("area do terreno : " + areaterreno);
            System.out.println("Valor total do pagamento: " + xf.format(financiamento.calcularTotalPagamento()));
            System.out.println("Valor total do mês: " + xf.format(financiamento.calcularpagamentomensal()));
            System.out.println();
        }

        // Coleta de dados de financiamento do Apartamento
        for (int i = 0; i < 2; i++) {
            double taxaJuros = interfaceUsuario.pedirTaxaJuros();
            int prazoFinanciamentoAnos = interfaceUsuario.pedirPrazoFinanciamento();
            double valorImovel = interfaceUsuario.pedirValorImovel();
            int numerodoandar = interfaceUsuario.pedirnumerodeandaro();
            int numerodevagasgaragem = interfaceUsuario.pedirnumerodevagasgaragem();
            Financiamento financiamento = new Apartamento(valorImovel, prazoFinanciamentoAnos, taxaJuros,numerodevagasgaragem,numerodoandar);
            listaDeFinanciamentos.add(financiamento);

            // Impressão dos detalhes do financiamento
            System.out.println("Financiamento do Apartamento" + (i + 1) + ":");
            System.out.println("Valor do imóvel Apartamento: " + valorImovel);
            System.out.println("Prazo de financiamento do Apartamento: " + prazoFinanciamentoAnos + " anos");
            System.out.println("Taxa de juros do Apartamento: " + taxaJuros + "%");
            System.out.println("numero vagas garagem: " + numerodevagasgaragem);
            System.out.println("numero andar : " + numerodoandar);
            System.out.println("Valor total do pagamento:: " + xf.format(financiamento.calcularTotalPagamento()));
            System.out.println("Valor total do mês: " + xf.format(financiamento.calcularpagamentomensal()));
            System.out.println();
        }

        // Coleta de dados de financiamento do terreno
        for (int i = 0; i < 1; i++) {
            double taxaJuros = interfaceUsuario.pedirTaxaJuros();
            int prazoFinanciamentoAnos = interfaceUsuario.pedirPrazoFinanciamento();
            double valorImovel = interfaceUsuario.pedirValorImovel();
            String tipozona = interfaceUsuario.pedirzona();
            Financiamento financiamento = new Terreno(valorImovel, prazoFinanciamentoAnos, taxaJuros,tipozona);
            listaDeFinanciamentos.add(financiamento);

            // Impressão dos detalhes do financiamento
            System.out.println("Financiamento terreno " + (i + 1) + ":");
            System.out.println("Valor do imóvel terreno: " + valorImovel);
            System.out.println("Prazo de financiamento do terreno: " + prazoFinanciamentoAnos + " anos");
            System.out.println("Taxa de juros do terreno: " + taxaJuros + "%");
            System.out.println("Valor do imóvel terreno: " + tipozona);
            System.out.println("Valor total do pagamento: " + xf.format(financiamento.calcularTotalPagamento()));
            System.out.println("Valor total do mês: " + xf.format(financiamento.calcularpagamentomensal()));
            System.out.println();
        }

    // Caminho do arquivo onde o ArrayList será salvo
        String caminhoArquivo = "meuArrayList2.ser";

        // Serializando o ArrayList
        try (FileOutputStream fileOut = new FileOutputStream(caminhoArquivo);
             ObjectOutputStream out = new ObjectOutputStream(fileOut)) {
            out.writeObject(listaDeFinanciamentos);
            System.out.println("ArrayList serializado e salvo em " + caminhoArquivo);
        } catch (IOException e) {
            e.printStackTrace();
        }

        ObjectInputStream inputStream = null;
        try {
            inputStream = new ObjectInputStream (new FileInputStream("meuArrayList2.ser"));
            Object obj = null;
            while ((obj = inputStream.readObject()) != null) {
                if (obj instanceof Financiamento) //
                {
                    System.out.println(((Financiamento)obj).toString());

                };

            }
            inputStream.close();
        } catch (EOFException ex) { // quando EOF (End Of File) é alçancado
            System.out.println("Fim de arquivo alcançado.");
        } catch (ClassNotFoundException ex) {
            ex.printStackTrace();
        } catch (FileNotFoundException ex) {
            ex.printStackTrace();
        } catch (IOException ex) {
            ex.printStackTrace();
        }


        // Cálculo dos totais e impressão dos resultados gerais
        double totalValorImovel = 0.0;
        double totalValorFinanciamento = 0.0;
        for (Financiamento financiamento : listaDeFinanciamentos) {
            totalValorImovel += financiamento.getValorImovel();
            totalValorFinanciamento += financiamento.calcularTotalPagamento();
        }

        // Impressão dos totais
        System.out.println("Total do valor dos imóveis: " + df.format(totalValorImovel));
        System.out.println("Total do valor dos financiamentos: " + df.format(totalValorFinanciamento));


    }
}

package util;
import java.util.Scanner;
import modelo.Financiamento;
import modelo.Casa;

import static java.lang.System.out;


public class InterfaceUsuario {
    private Scanner scanner = new Scanner(System.in);


   //if (taxadejuros <= 0 || taxadejuros >= 130);

    public double pedirTaxaJuros() {
        double taxadejuros = 0;
        try {
            System.out.print("Digite a taxa de juros: ");
            taxadejuros = scanner.nextDouble();


        } catch (Exception e) {
            System.out.println(e.getMessage()) ;
        }
        finally {
            System.out.println("------------------");
        }
        return taxadejuros;
    }




    public int pedirPrazoFinanciamento() {
        int PrazoFinanciamento = 0;
        try {
            out.print("Digite o prazo de financiamento (em anos): ");
            PrazoFinanciamento = scanner.nextInt();
            if (PrazoFinanciamento <= 0 || PrazoFinanciamento >= 130);

        } catch (Exception e) {
            System.out.println("ERRO DE DIGITAÇÃO") ;
        }
        finally {
            System.out.println("------------------");
        }
        return PrazoFinanciamento;
    }




    public double pedirValorImovel() {
        double ValorImovel = 0;
        try {
            out.print("Digite o valor do imóvel: ");
             ValorImovel = scanner.nextDouble();
            if (ValorImovel <= 0 || ValorImovel >= 130);

        } catch (Exception e) {
            System.out.println("ERRO DE DIGITAÇÃO") ;
        }
        finally {
            System.out.println("------------------");
        }
        return ValorImovel;
    }

    //casa
    public double pedirareaconstruida() {
        double areaconstruida = 0;
        try {
            out.print("Digite a area construida: ");
            areaconstruida = scanner.nextDouble();
            if (areaconstruida <= 0 || areaconstruida >= 130);

        } catch (Exception e) {
            System.out.println("ERRO DE DIGITAÇÃO") ;
        }
        finally {
            System.out.println("------------------");
        }
        return areaconstruida;
    }

    public double pedirareaterreno() {
        double areaterreno = 0;
        try {
            out.print("Digite a area do terreno: ");
            areaterreno = scanner.nextDouble();
            if (areaterreno <= 0 || areaterreno >= 130);

        } catch (Exception e) {
            System.out.println("ERRO DE DIGITAÇÃO") ;
        }
        finally {
            System.out.println("------------------");
        }
        return areaterreno;
    }

    //apartamento
    public int pedirnumerodeandaro() {
        int numerodeandaro = 0;
        try {
            out.print("Digite o numero do andar: ");
            numerodeandaro = scanner.nextInt();
            if (numerodeandaro <= 0 || numerodeandaro >= 130);

        } catch (Exception e) {
            System.out.println("ERRO DE DIGITAÇÃO") ;
        }
        finally {
            System.out.println("------------------");
        }
        return numerodeandaro;
    }

    public int pedirnumerodevagasgaragem() {
        int numerodevagasgaragem = 0;
        try {
            out.print("Digite de vagas na garagem: ");
            numerodevagasgaragem = scanner.nextInt();
            if (numerodevagasgaragem <= 0 || numerodevagasgaragem >= 130);

        } catch (Exception e) {
            System.out.println("ERRO DE DIGITAÇÃO") ;
        }
        finally {
            System.out.println("------------------");
        }
        return numerodevagasgaragem;
    }

    //terreno
    public String pedirzona() {

        String tipozona = null;
        try {
            out.print("qual zona seria  residencial ou comercial: ");
            tipozona = scanner.next();
        } catch (Exception e) {
            System.out.println("ERRO DE DIGITAÇÃO");
        } finally {
            System.out.println("------------------");
        }
        return tipozona;
    }
    }

    package util;

public class DescontoMaiorDoQueJurosException extends Exception{
    public DescontoMaiorDoQueJurosException(String msg){
        super(msg);
    }
}
package modelo;

import java.io.Serializable;

public abstract class  Financiamento implements Serializable {
    protected double valorImovel;
    protected int prazoFinanciamentoAnos;
    protected double taxaJuros;

    public Financiamento(double valorImovel, int prazoFinanciamentoAnos, double taxaJuros) {
        this.valorImovel = valorImovel;
        this.prazoFinanciamentoAnos = prazoFinanciamentoAnos;
        this.taxaJuros = taxaJuros;
    }

    public double getValorImovel() {
        return valorImovel;
    }

    public int getprazoFinanciamentoAnos() {
        return prazoFinanciamentoAnos;
    }

    public double gettaxaJuros() {
        return taxaJuros;
    }

    public void setValorImovel(double valorImovel) {
        this.valorImovel=valorImovel;
    }

    public void setTaxaJuros(double taxaJuros) {
        this.taxaJuros=taxaJuros;
    }


    //metodos
    public double calcularTotalPagamento() {
        return this.calcularpagamentomensal() * (double)this.prazoFinanciamentoAnos * 12.0;
    }


    public double calcularpagamentomensal() {
        return this.valorImovel / (double)(this.prazoFinanciamentoAnos * 12) * (1.0 + this.taxaJuros / 12.0);
    }
}
package modelo;
import java.io.*;
import java.io.Serializable;
import java.io.BufferedReader;
import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;
import java.io.PrintWriter;

import util.DescontoMaiorDoQueJurosException;

public class Casa extends Financiamento implements Serializable {
    private static final long serialVersionUID = 1L;

    private double areaconstruida;
    private double areaterreno;


    public Casa(double valorImovel, int prazoFinanciamentoAnos, double taxaJuros, double areaconstruida, double areaterreno) {
        super(valorImovel, prazoFinanciamentoAnos, taxaJuros);
        this.areaconstruida = areaconstruida;
        this.areaterreno = areaterreno;
    }

    public double getareaconstruida() {
        return areaconstruida;
    }

    public double getareaterreno() {
        return areaterreno;
    }

    private void validardesconto(double valorjuros, double valoracrescimo) throws DescontoMaiorDoQueJurosException {
        throw new DescontoMaiorDoQueJurosException("erro no juros");
    }

    public String toString() {
        // Agrupa o valor dos atributos em uma String
        StringBuilder builder = new StringBuilder();
        builder.append(this.calcularTotalPagamento()).append("\n");
        builder.append(this.calcularpagamentomensal()).append("\n");
        builder.append(this.getValorImovel()).append("\n");
        builder.append(this.gettaxaJuros()).append("\n");
        builder.append(this.getprazoFinanciamentoAnos()).append("\n");
        builder.append(this.getareaterreno()).append("\n");
        builder.append(this.getareaconstruida()).append("\n");
        return builder.toString();
    }

    public double calcularpagamentomensal() {

        double valorjuros = 40;
        double valoracrescimo = 80;
        try {
            validardesconto(valorjuros, valoracrescimo);
        } catch (DescontoMaiorDoQueJurosException e) {
            valorjuros = valoracrescimo;
        }
        return super.calcularpagamentomensal() + 80;
}
}
package modelo;

import java.io.Serializable;

import static java.lang.Math.pow;

public class Apartamento extends Financiamento implements Serializable {
    //construtor

    private int numerodevagasgaragem;
    private int numerodoandar;
    public Apartamento(double valorImovel, int prazoFinanciamentoAnos, double taxaJuros,int numerodevagasgaragem,int numerodoandar) {
        super(valorImovel, prazoFinanciamentoAnos, taxaJuros);
        this.numerodevagasgaragem=numerodevagasgaragem;
        this.numerodoandar=numerodoandar;
    }

    public int getnumerodevagasgaragem() {
        return numerodevagasgaragem;
    }

    public int getnumerodoandar() {
        return numerodoandar;
    }

    public double calcularpagamentomensal() {
        double taxamensal = taxaJuros / 12;
        int meses = prazoFinanciamentoAnos*12;
        double novaformula = valorImovel*pow(1+taxamensal,meses)/ pow(1+taxamensal,meses)-1;
        return novaformula;

    }
}
package modelo;

import java.io.Serializable;

public class Terreno extends Financiamento implements Serializable {
    //construtor

    private String tipozona;
    public Terreno(double valorImovel, int prazoFinanciamentoAnos, double taxaJuros,String tipozona) {
        super(valorImovel, prazoFinanciamentoAnos, taxaJuros);
        this.tipozona=tipozona;
    }

    public String gettipozona() {
        return tipozona;
    }

    public double calcularpagamentomensal() {
        return super.calcularpagamentomensal() * 1;

    }
}
