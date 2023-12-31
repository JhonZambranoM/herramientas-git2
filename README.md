# herramientas-git2

para hacer informes PDF
--SpringBoot
<div>
  # instalar la libreria OPENPDF <hr>
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
 spring.datasource.url=jdbc:mysql://localhost:3306/practicaconcurso?createDatabaseIfNotExist=true <br>
 spring.datasource.username=root <br>
spring.datasource.password= <br> 
 spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver <br>
 spring.jpa.database-platform=org.hibernate.dialect.MySQL8Dialect <br>
 spring.jpa.hibernate.ddl-auto=update <br>
 spring.jpa.show-sql=true <br>
 spring.jpa.properties.hibernate.format_sql=true <br>
 server.port=8080 <br>
 spring.mvc.contentnegotiation.favor-parameter=true <br>
 spring.mvc.contentnegotiation.media-types.pdf=application/pdf <br>

    </div>
  
</div>
##Fdfdf
constructor(private httpLibros:HttpClient) { }
  url = "http://localhost:8080/publicaciones";

  public guardarPublicacion(publicacion:Publicaciones):Observable<Object>{
    return this.httpLibros.post(this.url+"/guardar",publicacion);
  }

  //listar
  public listarTodas():Observable<Publicaciones[]>{
    return this.httpLibros.get<Publicaciones[]>(this.url+"/listar")
  }

  //treaer por id
  public listarPorId(id_publicaciones:number){
    this.httpLibros.get<Publicaciones>(this.url+id_publicaciones)
  }
  //actualizar
  public actualizarPublicacion(id_publicaciones:number, publicacion:Publicaciones):Observable<Object>{
    return this.httpLibros.put(this.url+"/actualizar/"+id_publicaciones,publicacion);
  }
 
  public eliminarPublicacion(id_publicaciones:number):Observable<Object>{
    return this.httpLibros.delete(this.url+"/eliminar/"+id_publicaciones);
  }



<div>
  
  publicaciones : Publicaciones = new Publicaciones();

  constructor(private pubServices: PublicacionesServicesService, private route:Router){}

  //guardar
  public guardarPublicacion(){
    this.pubServices.guardarPublicacion(this.publicaciones).subscribe({
      next:(datos)=>{
        this.irLista();
      },error:(error)=>{console.log(error)}
    });
  }

  irLista(){
    this.route.navigate(["/listar"])
  }
</div>

<div>
   public listarPublicaciones(){
    this.pubService.listarTodas().subscribe((datos)=>{
      console.log(datos);
      this.publicaciones = datos;
    });
  }

  public eliminar(id_publicaciones:number){
    this.pubService.eliminarPublicacion(id_publicaciones).subscribe({
      next:(datos)=>{this.irActualizar()},error:(error)=>{console.log("error al eliminar")}

    });
    
  }

  irActualizar(){
    this.route.navigate(["/"])
  }

</div>

## angular pdf
# servicio
  obtenerReportePDF():Observable<Blob>{
    // Configurar las cabeceras para indicar que esperamos un archivo PDF en la respuesta
    const headers = new HttpHeaders({ 'Content-Type': 'application/pdf' });

    // Realizar la solicitud GET al endpoint que genera el informe PDF
    return this.clienteHtpp.get(`${this.url}/generar-pdf`, { responseType: 'blob', headers });
  }

  # type
    descargarInformePDF() {
    this.librosService.obtenerReportePDF().subscribe(
      (data: Blob) => {
        const url = window.URL.createObjectURL(data);
        const a = document.createElement('a');
        a.href = url;
        a.download = 'informe.pdf';
        document.body.appendChild(a);
        a.click();
        document.body.removeChild(a);
        window.URL.revokeObjectURL(url);
      },
      (error) => {
        console.error('Error al descargar el informe PDF', error);
        // Maneja el error según tus necesidades
      }
    );
  }
# controller spring

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

    private void addTableHeader(PdfPTable table) {
        table.addCell("Título");
        table.addCell("Números de Páginas");
        table.addCell("ISBN");
        table.addCell("Fecha Creación");
        table.addCell("Fecha Modificación");
        table.addCell("Año de Publicación");
    }

    private void addRows(PdfPTable table, List<Libros> libros) {
        for (Libros libro : libros) {
            table.addCell(libro.getTitulo());
            table.addCell(String.valueOf(libro.getNum_pages()));
            table.addCell(libro.getIsbn());
            table.addCell(libro.getFecha_creacion().toString());
            table.addCell(libro.getFecha_modificacion().toString());
            table.addCell(String.valueOf(libro.getAnio_pub()));
        }
    }
