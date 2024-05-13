---
layout: post
title: "New chat"
tags: [New, chat]
---

## 사용자 입력:
> package com.ids.file.io;

import java.io.DataInput;
import java.io.DataInputStream;
import java.io.DataOutput;
import java.io.EOFException;
import java.io.File;
import java.io.FileDescriptor;
import java.io.FileNotFoundException;
import java.io.IOException;
import java.io.UTFDataFormatException;

/** 
 * A buffered drop-in replacement for java.io.RandomAccessFile.
 * Instances of this class realise substantial speed increases over
 * java.io.RandomAccessFile through the use of buffering. This is a
 * subclass of Object, as it was not possible to subclass
 * java.io.RandomAccessFile because many of the methods are
 * final. However, if it is necessary to use RandomAccessFile and
 * java.io.RandomAccessFile interchangeably, both classes implement the
 * DataInput and DataOutput interfaces.
 *
 * @author Alex McManus
 * @author Russ Rew
 * @see DataInput
 * @see DataOutput
 * @see java.io.RandomAccessFile
 */
public class RandomAccessFile implements DataOutput, DataInput {

	/** Read from the file. This is always implied. */
	private static final int READ = 1;
	
	/** Write to the file. */
	private static final int WRITE = 2;
	
	/** Create the file rather than overwriting it. This is ignored if mode is
	*  not also WRITE.
	*/
	private static final int CREATE = 4;
		
	/** The default buffer size, in bytes. */
    protected static final int defaultBufferSize = 4096;

    /** The underlying java.io.RandomAccessFile. */
    protected java.io.RandomAccessFile file;

    /** The offset in bytes from the file start, of the next read or
    *  write operation. */
    protected long filePosition;

    /** The buffer used to load the data. */
    protected byte buffer[];

   /** The offset in bytes of the start of the buffer, from the start
    *  of the file. */
   protected long bufferStart;

   /** The offset in bytes of the end of the data in the buffer, from
    *  the start of the file. This can be calculated from
    *  <code>bufferStart + dataSize</code>, but it is cached to speed
    *  up the read( ) method. */
   protected long dataEnd;

   /** The size of the data stored in the buffer, in bytes. This may be
    *  less than the size of the buffer.*/
   protected int dataSize;

   /** True if we are at the end of the file. */
   protected boolean endOfFile;

   /** The access mode of the file. This is a logical OR of READ,
    *  WRITE and CREATE. */
   protected int mode;

   /** True if the data in the buffer has been modified. */
   boolean bufferModified = false;
   
   /** Source File Object For File Control **/
   protected File sourceFile = null;

   /**
    * Create a new buffered random-access file with a default buffer size.
    * Note that the mode CREATE implies WRITE.
    *
    * @param filename  the name of the file.
    * @param mode      how the file is to be opened. This may be a
    *                  combination (logical OR) of CREATE, WRITE, and READ.
    * @exception IOException        if an I/O error occurrs.
    * @exception SecurityException  if a security manager exists, its checkRead
    *                               method is called with the name argument to
    *                               see if the application is allowed read
    *                               access to the file. If the mode argument is
    *                               WRITE, its checkWrite method also is called
    *                               with the name argument to see if the
    *                               application is allowed write access to the
    *                               file. Either of these may result in a
    *                               security exception.
    */
   public RandomAccessFile( String filename, int mode )
   throws IOException {
      this( filename, mode, defaultBufferSize );
   }

    /**
     * Creates a random access file stream to read from, and optionally 
     * to write to, a file with the specified name.
     * <p>
     * The mode argument must either be equal to <code>"r"</code> or 
     * <code>"rw"</code>, indicating that the file is to be opened for 
     * input only or for both input and output, respectively.  If the
     * mode is <code>"rw"</code> and the 
     * file does not exist, then an attempt is made to create it.
     *
     * @param      name   the system-dependent filename.
     * @param      mode   the access mode.
     * @exception  IllegalArgumentException  if the mode argument is not equal
     *               to <code>"r"</code> or to <code>"rw"</code>.
     * @exception  IOException               if an I/O error occurs.
     * @exception  SecurityException         if a security manager exists, its
     *               <code>checkRead</code> method is called with the name
     *               argument to see if the application is allowed read access
     *               to the file. If the mode argument is equal to
     *               <code>"rw"</code>, its <code>checkWrite</code> method also
     *               is called with the name argument to see if the application
     *               is allowed write access to the file. Either of these may
     *               result in a security exception.
     * @see        java.lang.SecurityException
     * @see        java.lang.SecurityManager#checkRead(java.lang.String)
     */
   public RandomAccessFile( String name, String mode )
       throws IOException {
              
       this( name, 
    		   mode.equals("r")? READ :
	     (mode.equals("rw")? WRITE | READ : 0),
	     defaultBufferSize );
   }

    /**
     * Creates a random access file stream to read from, and optionally 
     * to write to, a file with the specified name, using a specified 
     * buffer size. 
     * <p>
     * The mode argument must either be equal to <code>"r"</code> or 
     * <code>"rw"</code>, indicating that the file is to be opened for 
     * input only or for both input and output, respectively.  If the
     * mode is <code>"rw"</code> and the 
     * file does not exist, then an attempt is made to create it.
     *
     * @param      name   the system-dependent filename.
     * @param      mode   the access mode.
     * @param      buffferSize   the buffer size.
     * @exception  IllegalArgumentException  if the mode argument is not equal
     *               to <code>"r"</code> or to <code>"rw"</code>.
     * @exception  IOException               if an I/O error occurs.
     * @exception  SecurityException         if a security manager exists, its
     *               <code>checkRead</code> method is called with the name
     *               argument to see if the application is allowed read access
     *               to the file. If the mode argument is equal to
     *               <code>"rw"</code>, its <code>checkWrite</code> method also
     *               is called with the name argument to see if the application
     *               is allowed write access to the file. Either of these may
     *               result in a security exception.
     * @see        java.lang.SecurityException
     * @see        java.lang.SecurityManager#checkRead(java.lang.String)
     */
   public RandomAccessFile( String name, String mode, int buffferSize )
       throws IOException {
              
       this( name, 
    		   mode.equals("r")? READ :
	     (mode.equals("rw")? WRITE | READ : 0),
	     buffferSize);
   }
    
    /**
     * Creates a random access file stream to read from, and optionally 
     * to write to, the file specified by the <code>File</code> argument. 
     * A new {@link FileDescriptor} object is created to represent 
     * this file connection. 
     * <p>
     * The mode argument must either be equal to <code>"r"</code> or 
     * <code>"rw"</code>, indicating that the file is to be opened for 
     * input only or for both input and output, respectively. The 
     * write methods on this object will always throw an 
     * <code>IOException</code> if the file is opened with a mode of 
     * <code>"r"</code>. If the mode is <code>"rw"</code> and the 
     * file does not exist, then an attempt is made to create it.
     *
     * @param      file   the file object.
     * @param      mode   the access mode.
     * @exception  IllegalArgumentException  if the mode argument is not equal
     *               to <code>"r"</code> or to <code>"rw"</code>.
     * @exception  IOException               if an I/O error occurs.
     * @exception  SecurityException         if a security manager exists, its
     *               <code>checkRead</code> method is called with the pathname
     *               of the <code>File</code> argument to see if the
     *               application is allowed read access to the file. If the
     *               mode argument is equal to <code>"rw"</code>, its
     *               <code>checkWrite</code> method also is called with the
     *               pathname to see if the application is allowed write access
     *               to the file.
     * @see        java.io.File#getPath()
     * @see        java.lang.SecurityManager#checkRead(java.lang.String)
     */
    public RandomAccessFile(File file, String mode) throws IOException {
	this(file.getPath(), mode);
    }


    /**
     * Creates a random access file stream to read from, and optionally 
     * to write to, the file specified by the <code>File</code> argument.
     * A buffer size is specified.
     * A new {@link FileDescriptor} object is created to represent 
     * this file connection.
     * <p>
     * The mode argument must either be equal to <code>"r"</code> or 
     * <code>"rw"</code>, indicating that the file is to be opened for 
     * input only or for both input and output, respectively. The 
     * write methods on this object will always throw an 
     * <code>IOException</code> if the file is opened with a mode of 
     * <code>"r"</code>. If the mode is <code>"rw"</code> and the 
     * file does not exist, then an attempt is made to create it.
     *
     * @param      file   the file object.
     * @param      mode   the access mode.
     * @param      buffersize   the buffersize.
     * @exception  IllegalArgumentException  if the mode argument is not equal
     *               to <code>"r"</code> or to <code>"rw"</code>.
     * @exception  IOException               if an I/O error occurs.
     * @exception  SecurityException         if a security manager exists, its
     *               <code>checkRead</code> method is called with the pathname
     *               of the <code>File</code> argument to see if the
     *               application is allowed read access to the file. If the
     *               mode argument is equal to <code>"rw"</code>, its
     *               <code>checkWrite</code> method also is called with the
     *               pathname to see if the application is allowed write access
     *               to the file.
     * @see        java.io.File#getPath()
     * @see        java.lang.SecurityManager#checkRead(java.lang.String)
     */
    public RandomAccessFile(File file, String mode, int buffersize) 
	throws IOException {
	this(file.getPath(), mode, buffersize);
    }


   /**
    * Create a new buffered random-access file with a specified buffer
    * size. Note that the mode CREATE implies WRITE, and the READ is
    * always implied.
    *
    * @param filename    the name of the file.
    * @param mode        how the file is to be opened. This may be a
                         combination (logical OR) of CREATE, WRITE, and READ.
    * @param bufferSize  the size of the temporary buffer, in bytes.
    * @exception IOException        if an I/O error occurrs.
    * @exception SecurityException  if a security manager exists, its checkRead
    *                               method is called with the name argument to
    *                               see if the application is allowed read
    *                               access to the file. If the mode argument is
    *                               WRITE, its checkWrite method also is called
    *                               with the name argument to see if the
    *                               application is allowed write access to the
    *                               file. Either of these may result in a
    *                               security exception.
    */
   private RandomAccessFile( String filename, int mode, int bufferSize ) throws IOException {

      this.mode = mode;

      // If we are CREATEing a file, we must also WRITE. READ is always
      // set.
      mode |= READ;
      if( (this.mode & CREATE) > 0 )
         this.mode |= WRITE;

      // To match java.io.RandomAccessFile semantics, if we want to write 
      // a nonexistant file, create it first (even if CREATE not set)
      File checkfile = new File( filename );
      if((this.mode & WRITE) > 0 && !checkfile.exists( ) ) {
	  mode |= CREATE;
      }

      // If a new file is to be created, delete any existing file with the same name.
      if( (this.mode & CREATE) > 0 ) {
         if( checkfile.exists( ) ) {
            if( !checkfile.delete( ) )
               throw new IOException( "Failed to delete " + filename );
         }
      }

      // If only reading, check that the file exists.
      if( this.mode == READ && !(new File( filename )).exists( ) )
         throw new FileNotFoundException( filename );

      // Create the underlying file object.
      String modeString = ((this.mode & WRITE) > 0) ? "rw" : "r";
      file = new java.io.RandomAccessFile( filename, modeString );

      // Initialise the buffer;
      bufferStart = 0;
      dataEnd = 0;
      dataSize = 0;
      filePosition = 0;
      buffer = new byte[bufferSize];
      endOfFile = false;
      sourceFile = new File(filename); 
   }

   /**
    * Close the file, and release any associated system resources.
    *
    * @exception IOException  if an I/O error occurrs.
    */
   public void close()
   throws IOException {

      // If we are writing and the buffer has been modified, flush the contents
      // of the buffer.
      if( (mode | WRITE) > 0 && bufferModified ) {
         file.seek( bufferStart );
         file.write( buffer, 0, (int)dataSize );
      }

      // Close the underlying file object.
      file.close( );
   }

   /**
    * Set the position in the file for the next read or write.
    *
    * @param pos  the offset (in bytes) from the start of the file.
    * @exception IOException  if an I/O error occurrs.
    */
   public final void seek( long pos ) 
   throws IOException {

      // If the seek is into the buffer, just update the file pointer.
      if( pos >= bufferStart && pos < dataEnd ) {
         filePosition = pos;
         return;
      }

      // If the current buffer is modified, write it to disk.
      if( bufferModified ) {
         flush( );
      }

      // Move to the position on the disk.
      file.seek( pos );
      filePosition = file.getFilePointer( );
      bufferStart = filePosition;

      // Fill the buffer from the disk.
      dataSize = file.read( buffer );
      if( dataSize < 0 ) {
         dataSize = 0;
         endOfFile = true;
      } else {
         endOfFile = false;
      }

      // Cache the position of the buffer end.
      dataEnd = bufferStart + dataSize;
   }

   /**
    * Returns the current position in the file, where the next read or
    * write will occur.
    *
    * @return the offset from the start of the file in bytes.
    * @exception IOException  if an I/O error occurrs.
    */
   public final long getFilePointer()
   throws IOException {
      return filePosition;
   }

   /**
    * Get the length of the file. The data in the buffer (which may not
    * have been written the disk yet) is taken into account.
    *
    * @return the length of the file in bytes.
    * @exception IOException  if an I/O error occurrs.
    */
   public long length( )
   throws IOException {
      long fileLength = file.length( );
      if( fileLength < dataEnd )
         return dataEnd;
      else
         return fileLength;
   }

    /** 
     * Truncates or extends file to a different length.  If file
     * pointer is at end of file and it gets truncated, file pointer is
     * set to end of truncated file.
     *
     * @param newLength  the desired new length of the file.
     */
    public void setLength(long newLength) throws IOException {
	file.setLength(newLength);
    }


    /** 
     * Returns the opaque file descriptor object associated with this file.
     *
     * @return the file descriptor object associated with this file.
     * @exception IOException if an I/O error occurs.
     */
    public final FileDescriptor getFD()
	throws IOException {
	return file.getFD();
    }

   /**
    * Copy the contents of the buffer to the disk.
    *
    * @exception IOException  if an I/O error occurrs.
    */
   public void flush( )
   throws IOException {
      file.seek( bufferStart );
      file.write( buffer, 0, dataSize );
      bufferModified = false;
   }

   //
   // Read primitives.
   //

   /**
    * Read a byte of data from the file, blocking until data is
    * available.
    *
    * @return the next byte of data, or -1 if the end of the file is
    *         reached.
    * @exception IOException  if an I/O error occurrs.
    */
   public final int read() 
   throws IOException {

      // If the file position is within the data, return the byte...
      if( filePosition < dataEnd ) {
         return (int)(buffer[(int)(filePosition++ - bufferStart)] & 0xff);

      // ...or should we indicate EOF...
      } else if( endOfFile ) {
         return -1;

      // ...or seek to fill the buffer, and try again.
      } else {
         seek( filePosition );
         return read( );
      }
   }

   /**
    * Read up to <code>len</code> bytes into an array, at a specified
    * offset. This will block until at least one byte has been read.
    *
    * @param b    the byte array to receive the bytes.
    * @param off  the offset in the array where copying will start.
    * @param len  the number of bytes to copy.
    * @return the actual number of bytes read, or -1 if there is not
    *         more data due to the end of the file being reached.
    * @exception IOException  if an I/O error occurrs.
    */
   private int readBytes( byte b[], int off, int len )
   throws IOException {

      // Check for end of file.
      if( endOfFile )
         return -1;

      // See how many bytes are available in the buffer - if none,
      // seek to the file position to update the buffer and try again.
      int bytesAvailable = (int)(dataEnd - filePosition);
      if( bytesAvailable < 1 ) {
         seek( filePosition );
         return readBytes( b, off, len );
      }

      // Copy as much as we can.
      int copyLength = (bytesAvailable >= len) ? len : bytesAvailable;
      System.arraycopy( buffer, (int)(filePosition - bufferStart),
                        b, off, copyLength );
      filePosition += copyLength;

      // If there is more to copy...
      if( copyLength < len ) {
         int extraCopy = len - copyLength;

         // If the amount remaining is more than a buffer's length, read it
         // directly from the file.
         if( extraCopy > buffer.length ) {
            file.seek( filePosition );
            extraCopy = file.read( b, off + copyLength, len - copyLength );

         // ...or read a new buffer full, and copy as much as possible...
         } else {
            seek( filePosition );
            if( ! endOfFile ) {
               extraCopy = (extraCopy > dataSize) ? dataSize : extraCopy;
               System.arraycopy( buffer, 0, b, off + copyLength, extraCopy );
            } else {
               extraCopy = -1;
            }
         }

         // If we did manage to copy any more, update the file position and
         // return the amount copied.
         if( extraCopy > 0 ) {
            filePosition += extraCopy;
            return copyLength + extraCopy;
         }
      }

      // Return the amount copied.
      return copyLength;
   }

   /**
    * Read up to <code>len</code> bytes into an array, at a specified
    * offset. This will block until at least one byte has been read.
    *
    * @param b    the byte array to receive the bytes.
    * @param off  the offset in the array where copying will start.
    * @param len  the number of bytes to copy.
    * @return the actual number of bytes read, or -1 if there is not
    *         more data due to the end of the file being reached.
    * @exception IOException  if an I/O error occurrs.
    */
   public int read( byte b[], int off, int len )
   throws IOException {
      return readBytes( b, off, len );
   }

   /**
    * Read up to <code>b.length( )</code> bytes into an array. This
    * will block until at least one byte has been read.
    *
    * @param b  the byte array to receive the bytes.
    * @return the actual number of bytes read, or -1 if there is not
    *         more data due to the end of the file being reached.
    * @exception IOException  if an I/O error occurrs.
    */
   public int read( byte b[] )
   throws IOException {
      return readBytes( b, 0, b.length );
   }

   /**
    * Reads <code>b.length</code> bytes from this file into the byte 
    * array. This method reads repeatedly from the file until all the 
    * bytes are read. This method blocks until all the bytes are read, 
    * the end of the stream is detected, or an exception is thrown. 
    *
    * @param b  the buffer into which the data is read.
    * @exception EOFException  if this file reaches the end before reading
    *                          all the bytes.
    * @exception IOException   if an I/O error occurs.
    */
   public final void readFully( byte b[] )
   throws IOException {
      readFully(b, 0, b.length);
   }

   /**
    * Reads exactly <code>len</code> bytes from this file into the byte 
    * array. This method reads repeatedly from the file until all the 
    * bytes are read. This method blocks until all the bytes are read, 
    * the end of the stream is detected, or an exception is thrown. 
    *
    * @param      b     the buffer into which the data is read.
    * @param      off   the start offset of the data.
    * @param      len   the number of bytes to read.
    * @exception  EOFException  if this file reaches the end before reading
    *                           all the bytes.
    * @exception  IOException   if an I/O error occurs.
    */
   public final void readFully( byte b[], int off, int len )
   throws IOException {
      int n = 0;
      while (n < len) {
         int count = this.read(b, off + n, len - n);
         if (count < 0)
            throw new EOFException();
         n += count;
      }
   }

   /**
    * Skips exactly <code>n</code> bytes of input. 
    * This method blocks until all the bytes are skipped, the end of 
    * the stream is detected, or an exception is thrown. 
    *
    * @param      n   the number of bytes to be skipped.
    * @return the number of bytes skipped, which is always <code>n</code>.
    * @exception  EOFException  if this file reaches the end before skipping
    *                           all the bytes.
    * @exception  IOException   if an I/O error occurs.
    */
   public int skipBytes( int n )
   throws IOException {
      seek(getFilePointer() + n);
      return n;
   }

   /**
    * Unread the last byte read.
    * This method should not be used more than once
    * between reading operations, or strange things might happen.
    */
   public final void unread( ) {
      filePosition--;
   }

   //
   // Write primitives.
   //

   /**
    * Write a byte to the file. If the file has not been opened for
    * writing, an IOException will be raised only when an attempt is
    * made to write the buffer to the file.
    * <p>
    * Caveat: the effects of seek( )ing beyond the end of the file are
    *         undefined.
    *
    * @exception IOException if an I/O error occurrs.
    */
   public final void write( int b )
   throws IOException {

      // If the file position is within the block of data...
      if( filePosition < dataEnd ) {
         buffer[(int)(filePosition++ - bufferStart)] = (byte)b;
         bufferModified = true;

      // ...or (assuming that seek will not allow the file pointer
      // to move beyond the end of the file) get the correct block of
      // data... 
      } else {

         // If there is room in the buffer, expand it...
         if( dataSize != buffer.length ) {
            buffer[(int)(filePosition++ - bufferStart)] = (byte)b;
            bufferModified = true;
            dataSize++;
            dataEnd++;

         // ...or do another seek to get a new buffer, and start again...
         } else {
            seek( filePosition );
            write( b );
         }
      }
   }

   /**
    * Write <code>len</code> bytes from an array to the file.
    *
    * @param b    the array containing the data.
    * @param off  the offset in the array to the data.
    * @param len  the length of the data.
    * @exception IOException  if an I/O error occurrs.
    */
   public final void writeBytes( byte b[], int off, int len )
   throws IOException {

      // If the amount of data is small (less than a full buffer)...
      if( len < buffer.length ) {

         // If any of the data fits within the buffer...
         int spaceInBuffer = 0;
         int copyLength = 0;
         if( filePosition >= bufferStart )
            spaceInBuffer = (int)((bufferStart + buffer.length) - filePosition);
         if( spaceInBuffer > 0 ) {

            // Copy as much as possible to the buffer.
            copyLength = (spaceInBuffer > len) ? len : spaceInBuffer;
            System.arraycopy( b, off, buffer,
                              (int)(filePosition - bufferStart), copyLength );
            bufferModified = true;
            long myDataEnd = filePosition + copyLength;
            dataEnd = myDataEnd > dataEnd ? myDataEnd : dataEnd;
            dataSize = (int)(dataEnd - bufferStart);
            filePosition += copyLength;
         }

         // If there is any data remaining, move to the new position and copy to
         // the new buffer.
         if( copyLength < len ) {
            seek( filePosition );
            System.arraycopy( b, off + copyLength, buffer, 
                              (int)(filePosition - bufferStart), 
                              len - copyLength );
            bufferModified = true;
            long myDataEnd = filePosition + (len - copyLength);
            dataEnd = myDataEnd > dataEnd ? myDataEnd : dataEnd;
            dataSize = (int)(dataEnd - bufferStart);
            filePosition += (len - copyLength);
         }

      // ...or write a lot of data...
      } else {

         // Flush the current buffer, and write this data to the file.
         if( bufferModified ) {
            flush( );
            bufferStart = dataEnd = dataSize = 0;
         }
         file.write( b, off, len );
         filePosition += len;
      }
   }
   
   /**
    * Writes <code>b.length</code> bytes from the specified byte array 
    * starting at offset <code>off</code> to this file. 
    *
    * @param b   the data.
    * @exception IOException  if an I/O error occurs.
    */
   public void write(byte b[]) throws IOException {
      writeBytes(b, 0, b.length); 
   }

   /**
    * Writes <code>len</code> bytes from the specified byte array 
    * starting at offset <code>off</code> to this file. 
    *
    * @param b    the data.
    * @param off  the start offset in the data.
    * @param len  the number of bytes to write.
    * @exception IOException  if an I/O error occurs.
    */
   public void write(byte b[], int off, int len) throws IOException {
      writeBytes(b, off, len);
   }

   //
   // DataInput methods.
   //

   /**
    * Reads a <code>boolean</code> from this file. This method reads a 
    * single byte from the file. A value of <code>0</code> represents 
    * <code>false</code>. Any other value represents <code>true</code>. 
    * This method blocks until the byte is read, the end of the stream 
    * is detected, or an exception is thrown. 
    *
    * @return     the <code>boolean</code> value read.
    * @exception  EOFException  if this file has reached the end.
    * @exception  IOException   if an I/O error occurs.
    */
   public final boolean readBoolean() throws IOException {
      int ch = this.read();
      if (ch < 0)
         throw new EOFException();
      return (ch != 0);
   }

   /**
    * Reads a signed 8-bit value from this file. This method reads a 
    * byte from the file. If the byte read is <code>b</code>, where 
    * <code>0&nbsp;&lt;=&nbsp;b&nbsp;&lt;=&nbsp;255</code>, 
    * then the result is:
    * <ul><code>
    *     (byte)(b)
    *</code></ul>
    * <p>
    * This method blocks until the byte is read, the end of the stream 
    * is detected, or an exception is thrown. 
    *
    * @return     the next byte of this file as a signed 8-bit
    *             <code>byte</code>.
    * @exception  EOFException  if this file has reached the end.
    * @exception  IOException   if an I/O error occurs.
    */
   public final byte readByte() throws IOException {
      int ch = this.read();
      if (ch < 0)
         throw new EOFException();
      return (byte)(ch);
   }

   /**
    * Reads an unsigned 8-bit number from this file. This method reads 
    * a byte from this file and returns that byte. 
    * <p>
    * This method blocks until the byte is read, the end of the stream 
    * is detected, or an exception is thrown. 
    *
    * @return     the next byte of this file, interpreted as an unsigned
    *             8-bit number.
    * @exception  EOFException  if this file has reached the end.
    * @exception  IOException   if an I/O error occurs.
    */
   public final int readUnsignedByte() throws IOException {
      int ch = this.read();
      if (ch < 0)
         throw new EOFException();
      return ch;
   }

   /**
    * Reads a signed 16-bit number from this file. The method reads 2 
    * bytes from this file. If the two bytes read, in order, are 
    * <code>b1</code> and <code>b2</code>, where each of the two values is 
    * between <code>0</code> and <code>255</code>, inclusive, then the 
    * result is equal to:
    * <ul><code>
    *     (short)((b1 &lt;&lt; 8) | b2)
    * </code></ul>
    * <p>
    * This method blocks until the two bytes are read, the end of the 
    * stream is detected, or an exception is thrown. 
    *
    * @return     the next two bytes of this file, interpreted as a signed
    *             16-bit number.
    * @exception  EOFException  if this file reaches the end before reading
    *               two bytes.
    * @exception  IOException   if an I/O error occurs.
    */
   public final short readShort() throws IOException {
      int ch1 = this.read();
      int ch2 = this.read();
      if ((ch1 | ch2) < 0)
         throw new EOFException();
      return (short)((ch1 << 8) + (ch2 << 0));
   }

   /**
    * Reads an unsigned 16-bit number from this file. This method reads 
    * two bytes from the file. If the bytes read, in order, are 
    * <code>b1</code> and <code>b2</code>, where 
    * <code>0&nbsp;&lt;=&nbsp;b1, b2&nbsp;&lt;=&nbsp;255</code>, 
    * then the result is equal to:
    * <ul><code>
    *     (b1 &lt;&lt; 8) | b2
    * </code></ul>
    * <p>
    * This method blocks until the two bytes are read, the end of the 
    * stream is detected, or an exception is thrown. 
    *
    * @return     the next two bytes of this file, interpreted as an unsigned
    *             16-bit integer.
    * @exception  EOFException  if this file reaches the end before reading
    *               two bytes.
    * @exception  IOException   if an I/O error occurs.
    */
   public final int readUnsignedShort() throws IOException {
      int ch1 = this.read();
      int ch2 = this.read();
      if ((ch1 | ch2) < 0)
         throw new EOFException();
      return (ch1 << 8) + (ch2 << 0);
   }

   /**
    * Reads a Unicode character from this file. This method reads two
    * bytes from the file. If the bytes read, in order, are 
    * <code>b1</code> and <code>b2</code>, where 
    * <code>0&nbsp;&lt;=&nbsp;b1,&nbsp;b2&nbsp;&lt;=&nbsp;255</code>, 
    * then the result is equal to:
    * <ul><code>
    *     (char)((b1 &lt;&lt; 8) | b2)
    * </code></ul>
    * <p>
    * This method blocks until the two bytes are read, the end of the 
    * stream is detected, or an exception is thrown. 
    *
    * @return     the next two bytes of this file as a Unicode character.
    * @exception  EOFException  if this file reaches the end before reading
    *               two bytes.
    * @exception  IOException   if an I/O error occurs.
    */
   public final char readChar() throws IOException {
      int ch1 = this.read();
      int ch2 = this.read();
      if ((ch1 | ch2) < 0)
         throw new EOFException();
      return (char)((ch1 << 8) + (ch2 << 0));
   }

   /**
    * Reads a signed 32-bit integer from this file. This method reads 4 
    * bytes from the file. If the bytes read, in order, are <code>b1</code>,
    * <code>b2</code>, <code>b3</code>, and <code>b4</code>, where 
    * <code>0&nbsp;&lt;=&nbsp;b1, b2, b3, b4&nbsp;&lt;=&nbsp;255</code>, 
    * then the result is equal to:
    * <ul><code>
    *     (b1 &lt;&lt; 24) | (b2 &lt;&lt; 16) + (b3 &lt;&lt; 8) + b4
    * </code></ul>
    * <p>
    * This method blocks until the four bytes are read, the end of the 
    * stream is detected, or an exception is thrown. 
    *
    * @return     the next four bytes of this file, interpreted as an
    *             <code>int</code>.
    * @exception  EOFException  if this file reaches the end before reading
    *               four bytes.
    * @exception  IOException   if an I/O error occurs.
    */
   public final int readInt() throws IOException {
      int ch1 = this.read();
      int ch2 = this.read();
      int ch3 = this.read();
      int ch4 = this.read();
      if ((ch1 | ch2 | ch3 | ch4) < 0)
         throw new EOFException();
      return ((ch1 << 24) + (ch2 << 16) + (ch3 << 8) + (ch4 << 0));
   }

   /**
    * Reads a signed 64-bit integer from this file. This method reads eight
    * bytes from the file. If the bytes read, in order, are 
    * <code>b1</code>, <code>b2</code>, <code>b3</code>, 
    * <code>b4</code>, <code>b5</code>, <code>b6</code>, 
    * <code>b7</code>, and <code>b8,</code> where:
    * <ul><code>
    *     0 &lt;= b1, b2, b3, b4, b5, b6, b7, b8 &lt;=255,
    * </code></ul>
    * <p>
    * then the result is equal to:
    * <p><blockquote><pre>
    *     ((long)b1 &lt;&lt; 56) + ((long)b2 &lt;&lt; 48)
    *     + ((long)b3 &lt;&lt; 40) + ((long)b4 &lt;&lt; 32)
    *     + ((long)b5 &lt;&lt; 24) + ((long)b6 &lt;&lt; 16)
    *     + ((long)b7 &lt;&lt; 8) + b8
    * </pre></blockquote>
    * <p>
    * This method blocks until the eight bytes are read, the end of the 
    * stream is detected, or an exception is thrown. 
    *
    * @return     the next eight bytes of this file, interpreted as a
    *             <code>long</code>.
    * @exception  EOFException  if this file reaches the end before reading
    *               eight bytes.
    * @exception  IOException   if an I/O error occurs.
    */
   public final long readLong() throws IOException {
      return ((long)(readInt()) << 32) + (readInt() & 0xFFFFFFFFL);
   }

   /**
    * Reads a <code>float</code> from this file. This method reads an 
    * <code>int</code> value as if by the <code>readInt</code> method 
    * and then converts that <code>int</code> to a <code>float</code> 
    * using the <code>intBitsToFloat</code> method in class 
    * <code>Float</code>. 
    * <p>
    * This method blocks until the four bytes are read, the end of the 
    * stream is detected, or an exception is thrown. 
    *
    * @return     the next four bytes of this file, interpreted as a
    *             <code>float</code>.
    * @exception  EOFException  if this file reaches the end before reading
    *             four bytes.
    * @exception  IOException   if an I/O error occurs.
    * @see        java.io.RandomAccessFile#readInt()
    * @see        java.lang.Float#intBitsToFloat(int)
    */
   public final float readFloat() throws IOException {
      return Float.intBitsToFloat(readInt());
   }

   /**
    * Reads a <code>double</code> from this file. This method reads a 
    * <code>long</code> value as if by the <code>readLong</code> method 
    * and then converts that <code>long</code> to a <code>double</code> 
    * using the <code>longBitsToDouble</code> method in 
    * class <code>Double</code>.
    * <p>
    * This method blocks until the eight bytes are read, the end of the 
    * stream is detected, or an exception is thrown. 
    *
    * @return     the next eight bytes of this file, interpreted as a
    *             <code>double</code>.
    * @exception  EOFException  if this file reaches the end before reading
    *             eight bytes.
    * @exception  IOException   if an I/O error occurs.
    * @see        java.io.RandomAccessFile#readLong()
    * @see        java.lang.Double#longBitsToDouble(long)
    */
   public final double readDouble() throws IOException {
      return Double.longBitsToDouble(readLong());
   }

   /**
    * Reads the next line of text from this file. This method 
    * successively reads bytes from the file until it reaches the end of 
    * a line of text. 
    * <p>
    * A line of text is terminated by a carriage-return character 
    * (<code>'&#92;r'</code>), a newline character (<code>'&#92;n'</code>), a 
    * carriage-return character immediately followed by a newline 
    * character, or the end of the input stream. The line-terminating 
    * character(s), if any, are included as part of the string returned. 
    * <p>
    * This method blocks until a newline character is read, a carriage 
    * return and the byte following it are read (to see if it is a 
    * newline), the end of the stream is detected, or an exception is thrown.
    *
    * @return     the next line of text from this file.
    * @exception  IOException  if an I/O error occurs.
    */
   public final String readLine() throws IOException {
      StringBuffer input = new StringBuffer();
      int c;

      while (((c = read()) != -1) && (c != '\n')) {
         input.append((char)c);
      }
      if ((c == -1) && (input.length() == 0)) {
         return null;
      }
      return input.toString();
   }

   /**
    * Reads in a string from this file. The string has been encoded 
    * using a modified UTF-8 format. 
    * <p>
    * The first two bytes are read as if by 
    * <code>readUnsignedShort</code>. This value gives the number of 
    * following bytes that are in the encoded string, not
    * the length of the resulting string. The following bytes are then 
    * interpreted as bytes encoding characters in the UTF-8 format 
    * and are converted into characters. 
    * <p>
    * This method blocks until all the bytes are read, the end of the 
    * stream is detected, or an exception is thrown. 
    *
    * @return     a Unicode string.
    * @exception  EOFException            if this file reaches the end before
    *               reading all the bytes.
    * @exception  IOException             if an I/O error occurs.
    * @exception  UTFDataFormatException  if the bytes do not represent 
    *               valid UTF-8 encoding of a Unicode string.
    * @see        java.io.RandomAccessFile#readUnsignedShort()
    */
   public final String readUTF() throws IOException {
      return DataInputStream.readUTF(this);
   }

   //
   // DataOutput methods.
   //

   /**
    * Writes a <code>boolean</code> to the file as a 1-byte value. The 
    * value <code>true</code> is written out as the value 
    * <code>(byte)1</code>; the value <code>false</code> is written out 
    * as the value <code>(byte)0</code>.
    *
    * @param      v   a <code>boolean</code> value to be written.
    * @exception  IOException  if an I/O error occurs.
    */
   public final void writeBoolean(boolean v) throws IOException {
      write(v ? 1 : 0);
   }

   /**
    * Writes a <code>byte</code> to the file as a 1-byte value. 
    *
    * @param      v   a <code>byte</code> value to be written.
    * @exception  IOException  if an I/O error occurs.
    */
   public final void writeByte(int v) throws IOException {
      write(v);
   }

   /**
    * Writes a <code>short</code> to the file as two bytes, high byte first.
    *
    * @param      v   a <code>short</code> to be written.
    * @exception  IOException  if an I/O error occurs.
    */
   public final void writeShort(int v) throws IOException {
      write((v >>> 8) & 0xFF);
      write((v >>> 0) & 0xFF);
   }

   /**
    * Writes a <code>char</code> to the file as a 2-byte value, high
    * byte first.
    *
    * @param      v   a <code>char</code> value to be written.
    * @exception  IOException  if an I/O error occurs.
    */
   public final void writeChar(int v) throws IOException {
      write((v >>> 8) & 0xFF);
      write((v >>> 0) & 0xFF);
    }

   /**
    * Writes an <code>int</code> to the file as four bytes, high byte first.
    *
    * @param      v   an <code>int</code> to be written.
    * @exception  IOException  if an I/O error occurs.
    */
   public final void writeInt(int v) throws IOException {
      write((v >>> 24) & 0xFF);
      write((v >>> 16) & 0xFF);
      write((v >>>  8) & 0xFF);
      write((v >>>  0) & 0xFF);
   }

   /**
    * Writes a <code>long</code> to the file as eight bytes, high byte first.
    *
    * @param      v   a <code>long</code> to be written.
    * @exception  IOException  if an I/O error occurs.
    */
   public final void writeLong(long v) throws IOException {
      write((int)(v >>> 56) & 0xFF);
      write((int)(v >>> 48) & 0xFF);
      write((int)(v >>> 40) & 0xFF);
      write((int)(v >>> 32) & 0xFF);
      write((int)(v >>> 24) & 0xFF);
      write((int)(v >>> 16) & 0xFF);
      write((int)(v >>>  8) & 0xFF);
      write((int)(v >>>  0) & 0xFF);
   }

   /**
    * Converts the float argument to an <code>int</code> using the 
    * <code>floatToIntBits</code> method in class <code>Float</code>, 
    * and then writes that <code>int</code> value to the file as a 
    * 4-byte quantity, high byte first. 
    *
    * @param      v   a <code>float</code> value to be written.
    * @exception  IOException  if an I/O error occurs.
    * @see        java.lang.Float#floatToIntBits(float)
    */
   public final void writeFloat(float v) throws IOException {
      writeInt(Float.floatToIntBits(v));
   }

   /**
    * Converts the double argument to a <code>long</code> using the 
    * <code>doubleToLongBits</code> method in class <code>Double</code>, 
    * and then writes that <code>long</code> value to the file as an 
    * 8-byte quantity, high byte first. 
    *
    * @param      v   a <code>double</code> value to be written.
    * @exception  IOException  if an I/O error occurs.
    * @see        java.lang.Double#doubleToLongBits(double)
    */
   public final void writeDouble(double v) throws IOException {
      writeLong(Double.doubleToLongBits(v));
   }

   /**
    * Writes the string to the file as a sequence of bytes. Each 
    * character in the string is written out, in sequence, by discarding 
    * its high eight bits. 
    *
    * @param      s   a string of bytes to be written.
    * @exception  IOException  if an I/O error occurs.
    */
   public final void writeBytes(String s) throws IOException {
      int len = s.length();
      for (int i = 0 ; i < len ; i++) {
         write((byte)s.charAt(i));
      }
   }

   /**
    * Writes the character array to the file as a sequence of bytes. Each 
    * character in the string is written out, in sequence, by discarding 
    * its high eight bits. 
    *
    * @param b    a character array of bytes to be written.
    * @param off  the index of the first character to write.
    * @param len  the number of characters to write.
    * @exception  IOException  if an I/O error occurs.
    */
   public final void writeBytes( char b[], int off, int len ) throws IOException {
      for( int i = off; i < len; i++ ) {
         write( (byte)b[i] );
      }
   }

   /**
    * Writes a string to the file as a sequence of characters. Each 
    * character is written to the data output stream as if by the 
    * <code>writeChar</code> method. 
    *
    * @param      s   a <code>String</code> value to be written.
    * @exception  IOException  if an I/O error occurs.
    * @see        java.io.RandomAccessFile#writeChar(int)
    */
   public final void writeChars(String s) throws IOException {
      int len = s.length();
      for (int i = 0 ; i < len ; i++) {
         int v = s.charAt(i);
         write((v >>> 8) & 0xFF);
         write((v >>> 0) & 0xFF);
      }
   }

   /**
    * Writes a string to the file using UTF-8 encoding in a 
    * machine-independent manner. 
    * <p>
    * First, two bytes are written to the file as if by the 
    * <code>writeShort</code> method giving the number of bytes to 
    * follow. This value is the number of bytes actually written out, 
    * not the length of the string. Following the length, each character 
    * of the string is output, in sequence, using the UTF-8 encoding 
    * for each character. 
    *
    * @param      str   a string to be written.
    * @exception  IOException  if an I/O error occurs.
    */
   public final void writeUTF(String str) throws IOException {
      int strlen = str.length();
      int utflen = 0;

      for (int i = 0 ; i < strlen ; i++) {
         int c = str.charAt(i);
         if ((c >= 0x0001) && (c <= 0x007F)) {
            utflen++;
         } else if (c > 0x07FF) {
            utflen += 3;
         } else {
            utflen += 2;
         }
      }
      if (utflen > 65535)
         throw new UTFDataFormatException();		  

      write((utflen >>> 8) & 0xFF);
      write((utflen >>> 0) & 0xFF);
      for (int i = 0 ; i < strlen ; i++) {
         int c = str.charAt(i);
         if ((c >= 0x0001) && (c <= 0x007F)) {
            write(c);
         } else if (c > 0x07FF) {
            write(0xE0 | ((c >> 12) & 0x0F));
            write(0x80 | ((c >>  6) & 0x3F));
            write(0x80 | ((c >>  0) & 0x3F));
         } else {
            write(0xC0 | ((c >>  6) & 0x1F));
            write(0x80 | ((c >>  0) & 0x3F));
         }
      }
   }

   /**
    * Create a string representation of this object.
    *
    * @return a string representation of the state of the object.
    */
   public String toString( ) {
      return "fp=" + filePosition + ", bs=" + bufferStart +
             ", de=" + dataEnd + ", ds=" + dataSize +
             ", bl=" + buffer.length + ", m=" + mode +
             ", bm=" + bufferModified;
   }
   
   public void removeFile() {
	   this.sourceFile.delete();
   }


} 이 코드에서       // If a new file is to be created, delete any existing file with the same name.
      if( (this.mode & CREATE) > 0 ) {
         if( checkfile.exists( ) ) {
            if( !checkfile.delete( ) )
               throw new IOException( "Failed to delete " + filename );
         }
      } 이 부분이 TOCTOU_RACE_CONDITION 취약점이 나왔어. 수정해줘.

## ChatGPT 응답:
> The section of code you've highlighted is indeed vulnerable to a Time-of-Check to Time-of-Use (TOCTOU) race condition. This vulnerability arises because there's a gap of time between when the code checks for the file's existence ({% raw %}`checkfile.exists()`{% endraw %}) and when it actually deletes the file ({% raw %}`checkfile.delete()`{% endraw %}). During this gap, the state of the file system might change due to other processes' actions, leading to unintended behaviors or security issues.

To mitigate this TOCTOU vulnerability, you should avoid separating the check and the action. Instead of explicitly checking if the file exists and then deleting it, you should directly attempt to delete the file and handle any exceptions that occur if the file does not exist or cannot be deleted. This reduces the window where the file system's state can change between the check and the use.

Here's a revised version of your code:

{% raw %}```java
// Instead of checking if the file exists and then deleting, 
// directly attempt to delete the file.
try {
    if ((this.mode & CREATE) > 0) {
        Files.deleteIfExists(Paths.get(filename));
    }
} catch (IOException e) {
    throw new IOException("Failed to delete " + filename, e);
}
```{% endraw %}

In this revised version, I've used {% raw %}`Files.deleteIfExists`{% endraw %} from {% raw %}`java.nio.file.Files`{% endraw %}, which atomically checks for the file's existence and deletes it if present. This approach effectively handles the situation where the file might not exist (without throwing an exception) and properly throws an exception if the file exists but cannot be deleted for some reason. This is more robust and avoids the TOCTOU race condition.

## 사용자 입력:
> 한글로 설명해줘.

## ChatGPT 응답:
> 네, 문제가 된 코드 부분은 Time-of-Check to Time-of-Use (TOCTOU) 경쟁 조건, 즉 파일의 존재 여부를 확인하고 실제로 파일을 삭제하는 사이에 발생하는 취약점입니다. 이 경쟁 조건은 코드가 파일이 존재하는지 확인({% raw %}`checkfile.exists()`{% endraw %})한 후 실제로 파일을 삭제({% raw %}`checkfile.delete()`{% endraw %})할 때 사이에 발생합니다. 이 시간 간격 동안 파일 시스템의 상태가 다른 프로세스의 작업으로 인해 변경될 수 있어 예기치 않은 동작이나 보안 문제를 일으킬 수 있습니다.

이 TOCTOU 취약점을 완화하기 위해서는 확인과 행동을 분리하는 것을 피해야 합니다. 파일이 존재하는지 명시적으로 확인한 다음 삭제하는 대신, 파일을 직접 삭제하려고 시도하고 파일이 존재하지 않거나 삭제할 수 없는 경우 발생하는 예외를 처리해야 합니다. 이렇게 하면 파일 시스템의 상태가 확인과 사용 사이에 변경될 수 있는 창을 줄일 수 있습니다.

아래는 코드를 수정한 예시입니다:

{% raw %}```java
// 파일이 존재하는지 확인한 다음 삭제하는 대신,
// 직접 파일을 삭제하려고 시도합니다.
try {
    if ((this.mode & CREATE) > 0) {
        Files.deleteIfExists(Paths.get(filename));
    }
} catch (IOException e) {
    throw new IOException("Failed to delete " + filename, e);
}
```{% endraw %}

이 수정된 버전에서는 {% raw %}`java.nio.file.Files`{% endraw %}의 {% raw %}`Files.deleteIfExists`{% endraw %}를 사용하여 파일의 존재 여부를 자동으로 확인하고 존재할 경우 삭제합니다. 이 방법은 파일이 존재하지 않는 경우에도 예외를 발생시키지 않으면서 파일이 존재하지만 어떤 이유로 삭제할 수 없는 경우 적절히 예외를 발생시킵니다. 이는 더욱 견고하고 TOCTOU 경쟁 조건을 피하는 방법입니다.

