using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Hosting;
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;
using Microsoft.AspNetCore.Mvc;
using System.Collections.Generic;
using System.Threading.Tasks;

namespace BookstoreApi
{
 
    public class Book
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public int YearOfPublication { get; set; }
        public string AuthorName { get; set; }
    }

   
    public class BookstoreContext : DbContext
    {
        public BookstoreContext(DbContextOptions<BookstoreContext> options) : base(options) { }
        public DbSet<Book> Books { get; set; }
    }

  
    public class BookDto
    {
        public string Title { get; set; }
        public int YearOfPublication { get; set; }
        public string AuthorName { get; set; }
    }


    [ApiController]
    [Route("api/[controller]")]
    public class BooksController : ControllerBase
    {
        private readonly BookstoreContext _context;

        public BooksController(BookstoreContext context)
        {
            _context = context;
        }

        [HttpGet]
        public async Task<ActionResult<IEnumerable<Book>>> GetBooks()
        {
            return await _context.Books.ToListAsync();
        }

        [HttpGet("{id}")]
        public async Task<ActionResult<Book>> GetBook(int id)
        {
            var book = await _context.Books.FindAsync(id);
            if (book == null)
            {
                return NotFound();
            }
            return book;
        }

        [HttpPost]
        public async Task<ActionResult<Book>> PostBook(BookDto bookDto)
        {
            var book = new Book
            {
                Title = bookDto.Title,
                YearOfPublication = bookDto.YearOfPublication,
                AuthorName = bookDto.AuthorName
            };

            _context.Books.Add(book);
            await _context.SaveChangesAsync();

            return CreatedAtAction(nameof(GetBook), new { id = book.Id }, book);
        }

        [HttpPut("{id}")]
        public async Task<IActionResult> PutBook(int id, BookDto bookDto)
        {
            if (id != bookDto.Id)
            {
                return BadRequest();
            }

            var book = await _context.Books.FindAsync(id);
            if (book == null)
            {
                return NotFound();
            }

            book.Title = bookDto.Title;
            book.YearOfPublication = bookDto.YearOfPublication;
            book.AuthorName = bookDto.AuthorName;

            _context.Entry(book).State = EntityState.Modified;
            await _context.SaveChangesAsync();

            return NoContent();
        }

        [HttpDelete("{id}")]
        public async Task<IActionResult> DeleteBook(int id)
        {
            var book = await _context.Books.FindAsync(id);
            if (book == null)
            {
                return NotFound();
            }

            _context.Books.Remove(book);
            await _context.SaveChangesAsync();

            return NoContent();
        }
    }

   
    public class Program
    {
        public static void Main(string[] args)
        {
            var builder = WebApplication.CreateBuilder(args);

       
            builder.Services.AddDbContext<BookstoreContext>(options =>
                options.UseSqlite("Data Source=bookstore.db"));
            builder.Services.AddControllers();
            builder.Services.AddEndpointsApiExplorer();
            builder.Services.AddSwaggerGen();

            var app = builder.Build();

     
            if (app.Environment.IsDevelopment())
            {
                app.UseDeveloperExceptionPage();
                app.UseSwagger();
                app.UseSwaggerUI();
            }

            app.UseHttpsRedirection();
            app.UseAuthorization();
            app.MapControllers();

            app.Run();
        }
    }
}
