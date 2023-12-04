# herramientas-git2

para hacer informes PDF
--SpringBoot
<div>
  instalar la libreria OPENPDF
      @GetMapping("/generar-pdf")
    public ResponseEntity<byte[]> generarInformePDF() {
        List<Libros> libros = libroServicio.listarTodosLibros();

        ByteArrayOutputStream baos = new ByteArrayOutputStream();
        try {
            Document document = new Document();
            PdfWriter.getInstance(document, baos);
            document.open();

            PdfPTable table = new PdfPTable(6); // 6 columnas para las propiedades de Libros
            addTableHeader(table);
            addRows(table, libros);

            document.add(table);
            document.close();
        } catch (Exception e) {
            e.printStackTrace();
        }

        HttpHeaders headers = new HttpHeaders();
        headers.setContentType(MediaType.APPLICATION_PDF);
        headers.setContentDispositionFormData("inline", "informe.pdf");

        return new ResponseEntity<>(baos.toByteArray(), headers, HttpStatus.OK);
    }

    <div>
    spring.datasource.url=jdbc:mysql://localhost:3306/practicaconcurso?createDatabaseIfNotExist=true
spring.datasource.username=root
spring.datasource.password=
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.jpa.database-platform=org.hibernate.dialect.MySQL8Dialect
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true
server.port=8080
spring.mvc.contentnegotiation.favor-parameter=true
spring.mvc.contentnegotiation.media-types.pdf=application/pdf

    </div>
  
</div>
