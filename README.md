# Lab3
#
# Starting registers:
# $s0 = address of array A
# $s1 = number of elements in A
# $s2 = address of array P
# $s3 = current size of P


main:
    # P[0] = 1 because A[0]^0 = 1
    addi $t0, $zero, 1
    sw   $t0, 0($s2)

    # P now has one element
    addi $s3, $zero, 1

    # Start with index i = 1
    addi $t1, $zero, 1


loop:
    # Stop when i >= n
    slt  $t2, $t1, $s1
    beq  $t2, $zero, done

    # Find address of A[i]
    # Each integer is 4 bytes
    sll  $t3, $t1, 2
    add  $t4, $s0, $t3

    # Load A[i]
    lw   $a0, 0($t4)

    # Send index i as second argument
    add  $a1, $t1, $zero

    # Calculate A[i]^i
    jal  power

    # Set up arguments for newElement
    add  $a0, $s2, $zero
    add  $a1, $s3, $zero
    add  $a2, $v0, $zero

    # Add result to P
    jal  newElement

    # Increase size of P
    addi $s3, $s3, 1

    # Move to next index
    addi $t1, $t1, 1

    j loop


done:
    # $s3 now contains the final size of P
    # Program is finished
    j done



# ------------------------------------------------
# power
#
# $a0 = element (A[i])
# $a1 = index (i)
#
# Returns:
# $v0 = A[i]^i
# ------------------------------------------------

power:
    # Start result at 1
    addi $v0, $zero, 1

    # Counter starts at 0
    add  $t5, $zero, $zero


powerLoop:
    # Stop after multiplying index times
    slt  $t6, $t5, $a1
    beq  $t6, $zero, powerDone

    # result = result * element
    mult $v0, $a0
    mflo $v0

    # Increase counter
    addi $t5, $t5, 1

    j powerLoop


powerDone:
    jr $ra



# ------------------------------------------------
# newElement
#
# $a0 = base address of P
# $a1 = current size of P
# $a2 = new element
#
# Stores the new value at P[k]
# ------------------------------------------------

newElement:
    # Multiply current size by 4
    # to find the byte offset
    sll  $t7, $a1, 2

    # Find address of P[k]
    add  $t8, $a0, $t7

    # Store new value
    sw   $a2, 0($t8)

    jr $ra
