1.Moving Ball

public float X { get; set; }
public float Y { get; set; }

public float Radius { get; set; }

public float Velocity { get; set; }
public float Angle { get; set; }

public Rectangle Bounds;
        
private float velocityX;
private float velocityY;

public Ball(float x, float y, float radius, float velocity, float angle)
{
    X = x;
    Y = y;
    Radius = radius;
    Velocity = velocity;
    Angle = angle;
    velocityX = (float)Math.Cos(Angle) * Velocity;
    velocityY = (float)Math.Sin(Angle) * Velocity;
}


public void Move()
{
    float nextX = X + velocityX;
    float nextY = Y + velocityY;
    if (nextX - Radius <= Bounds.Left || (nextX + Radius >= Bounds.Right))
    {
        velocityX = -velocityX;
    }
    if (nextY - Radius <= Bounds.Top || (nextY + Radius >= Bounds.Bottom))
    {
        velocityY = -velocityY;
    }
    X += velocityX;
    Y += velocityY;
}


public void Draw(Brush brush, Graphics g)
{
    g.FillEllipse(brush, X - Radius, Y - Radius, Radius * 2, Radius * 2);
}


Timer timer;
   Ball ball;
   Graphics graphics;
   Brush brush;
   Pen pen;
   Rectangle bounds;
   Bitmap doubleBuffer;
   static readonly int FRAMES_PER_SECOND = 30;
   public Form1()
   {
        InitializeComponent();
        bounds = new Rectangle(10, 10, this.Bounds.Width - 40, this.Bounds.Height - 60);
        doubleBuffer = new Bitmap(Width, Height);
        graphics = CreateGraphics();
        ball = new Ball(Width / 2, Height / 2, 20, 10, (float)(Math.PI / 4));
        ball.Bounds = bounds;
        Show();
        brush = new SolidBrush(Color.Blue);
        pen = new Pen(Color.Red);
        timer = new Timer();
        timer.Tick += new EventHandler(timer_Tick);
        timer.Interval = 1000 / FRAMES_PER_SECOND;
        timer.Start();
    }

     void timer_Tick(object sender, EventArgs e)
     {
         Graphics g = Graphics.FromImage(doubleBuffer);
         g.Clear(Color.White);
         g.DrawRectangle(pen, bounds);
         ball.Draw(brush, g);
         ball.Move();
         graphics.DrawImageUnscaled(doubleBuffer, 0, 0);
     }




2.Circle
public override void Draw(System.Drawing.Graphics   g)
        {
            Brush b = new SolidBrush(Color);
            if (Selected)
            {
                Pen pen = new Pen(Color.Yellow);
                g.DrawEllipse(pen, Position.X - radius, Position.Y - radius, radius * 2, radius * 2);
                pen.Dispose();
            }
            g.FillEllipse(b, Position.X - radius, Position.Y - radius, radius * 2, radius * 2);
            b.Dispose();
        }



Public override void Pulse(int percent)
	        {
            if (isBlowing)
            {
                radius += 3;
                if (radius >= RADIUS * (1 + percent / 100.0))
                {
                    isBlowing = false;
                }
            }
            else
            {
                radius -= 3;
                if (radius <= RADIUS)
                {
                    isBlowing = true;
                }
            }
        }
3.Move shape
// On mouse move
	        public void MoveShape(Point position)
	        {
	            int dx = position.X - previousPoint.X;
	            int dy = position.Y - previousPoint.Y;
	            selectedShape.Position = new Point(
	                selectedShape.Position.X + dx,
	                selectedShape.Position.Y + dy);
	            previousPoint = position;
	        }


4. Move drawing
Vo LinesDoc
Public void Move(int dx, int dy)
	 {	
            foreach (Line line in lines)	
            {	
                line.Start = new Point(line.Start.X + dx, 	
                    line.Start.Y + dy);	
                line.End = new Point(line.End.X + dx, 	
                    line.End.Y + dy);	
                previousLocation = line.End;	
            }	
        }	

private void undoToolStripMenuItem_Click(object sender, EventArgs e)
       {
            document.Undo();
            Invalidate(true);
        }



Vo Form1

LinesDocument document;
private void Form1_KeyDown(object sender, KeyEventArgs e)
	  {
            if (e.KeyCode == Keys.Up)
            {
                document.Move(0, -5);
            }
            else if (e.KeyCode == Keys.Down)
            {
                document.Move(0, 5);
            }
            else if (e.KeyCode == Keys.Left)
            {
                document.Move(-5, 0);
            }
            else if (e.KeyCode == Keys.Right)
            {
                document.Move(5, 0);
            }
            Invalidate();
        }


5.Save File
private void saveFile()
        {	
            if (FileName == null)	
            {	
                SaveFileDialog saveFileDialog = new SaveFileDialog();	
                saveFileDialog.Filter = "Balloons doc file (*.bal)|*.bal";	
                saveFileDialog.Title = "Save balloons doc";	
                if (saveFileDialog.ShowDialog() == DialogResult.OK)	
                {	
                    FileName = saveFileDialog.FileName;	
                }	
            }	
            if (FileName != null)	
            {	
                using (FileStream fileStream = new FileStream(FileName, FileMode.Create))	
                {                    	
                    IFormatter formatter = new BinaryFormatter();	
                    formatter.Serialize(fileStream, balloonsDoc);	
                }	
            }	
        }	

6.Open FIle
private void openFile()
	        {
	            OpenFileDialog openFileDialog = new OpenFileDialog();
	            openFileDialog.Filter = "Balloons doc file (*.bal)|*.bal";
	            openFileDialog.Title = "Open balloons file";
	            if (openFileDialog.ShowDialog() == DialogResult.OK)
	            {
	                FileName = openFileDialog.FileName;
	                try
	                {
	                    using (FileStream fileStream = new FileStream(FileName, FileMode.Open))
	                    {
	                        IFormatter formater = new BinaryFormatter();
	                        balloonsDoc = (BalloonsDoc)formater.Deserialize(fileStream);
	                    }
	                }
	                catch (Exception ex)
	                {
	                    MessageBox.Show("Could not read file: " + FileName);
	                    FileName = null;
	                    return;
	                }
	                Invalidate(true);
	            }
	        }



7. Form-Closing
private void Form1_FormClosing(object sender, FormClosingEventArgs e)
    {
            DialogResult result = MessageBox.Show("Save document and exit", "Save document?", MessageBoxButtons.YesNoCancel);
            if (result == System.Windows.Forms.DialogResult.Cancel)
            {
                e.Cancel = true;
            }
            if (result == System.Windows.Forms.DialogResult.Yes)
            {
                saveFile();
            }
        }


8.Positioner
private void Form1_Paint(object sender, PaintEventArgs e)
	
	
 {
    e.Graphics.Clear(Color.White);
    if (hasPosioner)
    {
       e.Graphics.DrawLine(dashedPen, currentPoint.X, 0, currentPoint.X, Height);
       e.Graphics.DrawLine(dashedPen, 0, currentPoint.Y, Width, currentPoint.Y);
            }
            linesDoc.DrawLines(e.Graphics);
        
}

dashedPen = new Pen(Color.Black, 1);
	            
dashedPen.DashStyle = System.Drawing.Drawing2D.DashStyle.Dot;


