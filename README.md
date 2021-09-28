/*
 * To change this license header, choose License Headers in Project Properties.
 * To change this template file, choose Tools | Templates
 * and open the template in the editor.
 */
package logica;

import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.ArrayList;
import java.util.List;
import java.sql.Timestamp;
import persistencia.ConexionBD;

/**
 *
 * @author JerezBurgos
 */
public class Ocupacion {    
    private int idOcupacion;
    private String idPlaza;
    private String placa;
    Long fechaHora = System.currentTimeMillis();
    Timestamp fechaInicio = new Timestamp(fechaHora);
    Timestamp fechaFin = new Timestamp(fechaHora);    
    private int estado;
    private String factura;        

    public int getIdOcupacion() {
        return idOcupacion;
    }

    public void setIdOcupacion(int idOcupacion) {
        this.idOcupacion = idOcupacion;
    }

    public String getIdPlaza() {
        return idPlaza;
    }

    public void setIdPlaza(String idPlaza) {
        this.idPlaza = idPlaza;
    }

    public String getPlaca() {
        return placa;
    }

    public void setPlaca(String placa) {
        this.placa = placa;
    }

    public Timestamp getFechaInicio() {
        return fechaInicio;
    }

    public void setFechaInicio(Timestamp FechaInicio) {
        this.fechaInicio = fechaInicio;
    }

    public Timestamp getFechaFin() {
        return fechaFin;
    }

    public void setFechaFin(Timestamp FechaFin) {
        this.fechaFin = fechaFin;
    }
        
    public int getEstado() {
        return estado;
    }

    public void setEstado(int estado) {
        this.estado = estado;
    }

    public String getFactura() {
        return factura;
    }

    public void setFactura(String factura) {
        this.factura = factura;
    }
    
    @Override
    public String toString() {
        return "Heart{" + "idOcupacion=" + idOcupacion + ", idPlaza=" + idPlaza + ", Placa=" + placa
                + ", fechaInicio=" + fechaInicio + ", fechaFin=" + fechaFin
                + ", estado=" + estado + ", factura=" + factura + '}';
    }

    public List<Ocupacion> listarOcupacion() {
        List<Ocupacion> lista = new ArrayList<>();
        ConexionBD conexion = new ConexionBD();
        String sql = "select * from ocupacion;";
        try {
            ResultSet rs = conexion.consultarBD(sql);
            Ocupacion o;
            while (rs.next()) {
                o = new Ocupacion();
                o.setIdOcupacion(rs.getInt("idOcupacion"));
                o.setIdPlaza(rs.getString("idPlaza"));
                o.setPlaca(rs.getString("Placa"));
                o.setFechaInicio(rs.getTimestamp("FechaInicio"));
                o.setFechaFin(rs.getTimestamp("FechaFin"));
                o.setEstado(rs.getInt("Estado"));
                o.setFactura(rs.getString("Factura"));                
                lista.add(o);
            }
        } catch (SQLException ex) {
            System.out.println("Error: " + ex.getMessage());
        } finally {
            conexion.cerrarConexion();
        }
        return lista;
    }

    public boolean guardarOcupacion() {
        ConexionBD conexion = new ConexionBD();
        String sql = "insert into hearts (idOcupacion,idPlaza,Placa,FechaInicio,FechaFin,Estado,Factura)\n"
                + "values(" + this.idOcupacion + ",'" + this.idPlaza + "','" + this.placa
                + "'" + this.fechaInicio + "','" + this.fechaFin + "'," 
                + this.estado + ",'" + this.factura + "');";
        if (conexion.setAutoCommitBD(false)) {
            if (conexion.insertarBD(sql)) {
                conexion.commitBD();
                conexion.cerrarConexion();
                return true;
            } else {
                conexion.rollbackBD();
                conexion.cerrarConexion();
                return false;
            }
        } else {
            conexion.cerrarConexion();
            return false;
        }
    }

    public boolean actualizarOcupacion() {
        ConexionBD conexion = new ConexionBD();
        String sql = "update ocupacion set idOcupacion =" + this.idOcupacion 
                + ",idPlaza ='" + this.idPlaza 
                + "',Placa ='" + this.placa
                + "',FechaInicio ='" + this.fechaInicio + "',FechaFin ='" + this.fechaFin 
                + "'Estado =" + this.estado + ",Factura ='" + this.factura + "';";
        if (conexion.setAutoCommitBD(false)) {
            if (conexion.actualizarBD(sql)) {
                conexion.commitBD();
                conexion.cerrarConexion();
                return true;
            } else {
                conexion.rollbackBD();
                conexion.cerrarConexion();
                return false;
            }
        } else {
            conexion.cerrarConexion();
            return false;
        }
    }
    public boolean eliminarOcupacion() {
        ConexionBD conexion = new ConexionBD();
        String sql = "delete from ocupacion where serial="+this.idOcupacion+";";
        if (conexion.setAutoCommitBD(false)) {
            if (conexion.actualizarBD(sql)) {
                conexion.commitBD();
                conexion.cerrarConexion();
                return true;
            } else {
                conexion.rollbackBD();
                conexion.cerrarConexion();
                return false;
            }
        } else {
            conexion.cerrarConexion();
            return false;
        }
    }
}
